uptux/uptux.py at master · initstring/uptux · GitHub

|     |     |
| --- | --- |
| 1   | #!/usr/bin/env python |
| 2   |     |
| 3   | """ |
| 4   | uptux by initstring (gitlab.com/initstring) |
| 5   |     |
| 6   | This tool checks for configuration issues on Linux systems that may lead to |
| 7   | privilege escalation. |
| 8   |     |
| 9   | All functionality is contained in a single file, because installing packages |
| 10  | in restricted shells is a pain. |
| 11  | """ |
| 12  |     |
| 13  |     |
| 14  | import  os |
| 15  | import  sys |
| 16  | import  socket |
| 17  | import  getpass |
| 18  | import  argparse |
| 19  | import  datetime |
| 20  | import  subprocess |
| 21  | import  inspect |
| 22  | import  glob |
| 23  | import  re |
| 24  |     |
| 25  |     |
| 26  | BANNER  =  r''' |
| 27  |     |
| 28  |     |
| 29  |     |
| 30  | ____ ___ ___________ |
| 31  | \| \| \_____\__ ___/_ _____ ___ |
| 32  | \| \| /\____ \\| \| \| \| \ \/ / |
| 33  | \| \| / \| \|_> > \| \| \| /> < |
| 34  | \|______/ \| __/\|____\| \|____//__/\_ \ |
| 35  | \|__\| \/ |
| 36  |     |
| 37  |     |
| 38  |     |
| 39  | PrivEsc for modern Linux systems |
| 40  | github.com/initstring/uptux |
| 41  |     |
| 42  |     |
| 43  | ''' |
| 44  |     |
| 45  |     |
| 46  | ########################## Global Declarations Follow ######################### |
| 47  |     |
| 48  | LOGFILE  =  'log-uptux-{:%Y-%m-%d-%H.%M.%S}'.format(datetime.datetime.now()) |
| 49  | PARSER  =  argparse.ArgumentParser(description= |
| 50  |  "PrivEsc for modern Linux systems," |
| 51  |  " by initstring (gitlab.com/initstring)") |
| 52  | PARSER.add_argument('-n', '--nologging', action='store_true', |
| 53  |  help='do not write output to a logfile') |
| 54  | PARSER.add_argument('-d', '--debug', action='store_true', |
| 55  |  help='print some debugging info to the console') |
| 56  | ARGS  =  PARSER.parse_args() |
| 57  |     |
| 58  | ## Known directories for storing systemd files. |
| 59  | SYSTEMD_DIRS  = ['/etc/systemd/**/', |
| 60  |  '/lib/systemd/**/', |
| 61  |  '/run/systemd/**/', |
| 62  |  '/usr/lib/systemd/**/'] |
| 63  |     |
| 64  | ## Known directories for storing D-Bus configuration files |
| 65  | DBUS_DIRS  = ['/etc/dbus-1/system.d/', |
| 66  |  '/etc/dbus-1/session.d'] |
| 67  |     |
| 68  | # Target files that we know we cannot exploit |
| 69  | NOT_VULN  = ['/dev/null', |
| 70  |  '.'] |
| 71  |     |
| 72  | # Used to enable/disable the relative path checks of systemd |
| 73  | SYSTEMD_PATH_WRITABLE  =  False |
| 74  | ########################## End of Global Declarations ######################### |
| 75  |     |
| 76  |     |
| 77  | ############################ Setup Functions Follow ########################### |
| 78  |     |
| 79  | # This is the place for functions that help set up the application. |
| 80  |     |
| 81  | def  tee(text, **kwargs): |
| 82  |  """Used to log and print concurrently""" |
| 83  |     |
| 84  |  # Defining variables to print color-coded messages to the console. |
| 85  |  colors  = {'green': '\033[92m', |
| 86  |  'blue': '\033[94m', |
| 87  |  'orange': '\033[93m', |
| 88  |  'red': '\033[91m',} |
| 89  |  end_color  =  '\033[0m' |
| 90  |  boxes  = {'ok': colors['blue'] +  '[*] '  +  end_color, |
| 91  |  'note': colors['green'] +  '[+] '  +  end_color, |
| 92  |  'warn': colors['orange'] +  '[!] '  +  end_color, |
| 93  |  'vuln': colors['red'] +  '[VULNERABLE] '  +  end_color, |
| 94  |  'sus': colors['orange'] +  '[INVESTIGATE] '  +  end_color} |
| 95  |     |
| 96  |  # If this function is called with an optional 'box=xxx' parameter, these |
| 97  |  # will be prepended to the message. |
| 98  |  box  =  kwargs.get('box', '') |
| 99  |  if  box: |
| 100 |  box  =  boxes[box] |
| 101 |     |
| 102 |  # First, just print the item to the console. |
| 103 |  print(box  +  text) |
| 104 |     |
| 105 |  # Then, write it to the log if logging is not disabled |
| 106 |  if  not  ARGS.nologging: |
| 107 |  try: |
| 108 |  with  open(LOGFILE, 'a') as  logfile: |
| 109 |  logfile.write(box  +  text  +  '\n') |
| 110 |  except  PermissionError: |
| 111 |  ARGS.nologging  =  True |
| 112 |  print(boxes['warn'] +  "Could not create a log file due to" |
| 113 |  " insufficient permissions. Continuing with checks...") |
| 114 |     |
| 115 |     |
| 116 | def  check_handler(check, check_name, check_desc): |
| 117 |  """Check handler |
| 118 |     |
| 119 | This function takes a dictionary of check_desc,check_name and will |
| 120 | iterate through them all. |
| 121 | """ |
| 122 |  tee("\n\n++++++++++ {}: {} ++++++++++\n\n" |
| 123 | .format(check_name, check_desc)) |
| 124 |  tee("Starting module at {:%Y-%m-%d-%H.%M.%S}" |
| 125 | .format(datetime.datetime.now()), box='ok') |
| 126 |  tee("\n") |
| 127 |  check() |
| 128 |  tee("\n") |
| 129 |  tee("Finished module at {:%Y-%m-%d-%H.%M.%S}\n" |
| 130 | .format(datetime.datetime.now()), box='ok') |
| 131 |     |
| 132 |     |
| 133 | def  get_function_order(function): |
| 134 |  """Helper function for build_checks_list""" |
| 135 |  # Grabs the line number of a function it is passed. |
| 136 |  order  =  function.__code__.co_firstlineno |
| 137 |  return  order |
| 138 |     |
| 139 |     |
| 140 | def  build_checks_list(): |
| 141 |  """Dynamically build list of checks to execute |
| 142 |     |
| 143 | This function will grab, in order, all functions that start with |
| 144 | 'uptux_check_' and populate a list. This is then used to run the checks. |
| 145 | """ |
| 146 |  # Start to build a list of functions we will execute. |
| 147 |  uptux_checks  = [] |
| 148 |     |
| 149 |  # Get the name of this python script and all the functions inside it. |
| 150 |  current_module  =  sys.modules[__name__] |
| 151 |  all_functions  =  inspect.getmembers(current_module, inspect.isfunction) |
| 152 |     |
| 153 |  # If the function name matches 'uptux_check_' we will include it. |
| 154 |  for  function  in  all_functions: |
| 155 |  function_name  =  function[0] |
| 156 |  function_object  =  function[1] |
| 157 |  if  'uptux_check_'  in  function_name: |
| 158 |  uptux_checks.append(function_object) |
| 159 |     |
| 160 |  # Use the helper function to sort by line number in script. |
| 161 |  uptux_checks.sort(key=get_function_order) |
| 162 |     |
| 163 |  # Return the sorted list of functions. |
| 164 |  return  uptux_checks |
| 165 |     |
| 166 | ############################# End Setup Functions ############################# |
| 167 |     |
| 168 |     |
| 169 | ########################### Helper Functions Follow ########################### |
| 170 |     |
| 171 | # This is the place to put functions that are used by multiple "Individual |
| 172 | # Checks" (those starting with uptux_check_). |
| 173 |     |
| 174 | def  shell_exec(command): |
| 175 |  """Executes Linux shell commands""" |
| 176 |  # Split the command into a list as needed by subprocess |
| 177 |  command  =  command.split() |
| 178 |     |
| 179 |  # Get both stdout and stderror from command. Grab the Python exception |
| 180 |  # if there is one. |
| 181 |  try: |
| 182 |  out_bytes  =  subprocess.check_output(command, |
| 183 |  stderr=subprocess.STDOUT) |
| 184 |  except  subprocess.CalledProcessError  as  error: |
| 185 |  out_bytes  =  error.output |
| 186 |  except  OSError  as  error: |
| 187 |  print('Could not run the following OS command. Sorry!\n' |
| 188 |  ' Command: {}'.format(command)) |
| 189 |  print(error) |
| 190 |  sys.exit() |
| 191 |     |
| 192 |  # Return the lot as a text string for processing. |
| 193 |  out_text  =  out_bytes.decode('utf-8') |
| 194 |  out_text  =  out_text.rstrip() |
| 195 |  return  out_text |
| 196 |     |
| 197 |     |
| 198 | def  find_system_files(**kwargs): |
| 199 |  """Locates system files |
| 200 |     |
| 201 | Expected kwargs: known_dirs, search_mask |
| 202 | """ |
| 203 |     |
| 204 |  # Define known Linux folders for storing service unit definitions |
| 205 |  return_list  =  set() |
| 206 |     |
| 207 |  # Recursively gather all service unit from the known directories |
| 208 |  # and add them to a deduplicated set. |
| 209 |  for  directory  in  kwargs['known_dirs']: |
| 210 |  found_files  =  glob.glob(directory  +  kwargs['search_mask']) |
| 211 |  for  item  in  found_files: |
| 212 |  # We don't care about files that point to /dev/null. |
| 213 |  if  '/dev/null'  not  in  os.path.realpath(item): |
| 214 |  return_list.add(item) |
| 215 |     |
| 216 |  if  ARGS.debug: |
| 217 |  print("DEBUG, FOUND FILES") |
| 218 |  for  item  in  return_list: |
| 219 |  print(item) |
| 220 |     |
| 221 |  return  return_list |
| 222 |     |
| 223 |     |
| 224 | def  regex_vuln_search(**kwargs): |
| 225 |  """Helper function for searching text files |
| 226 |     |
| 227 | This function will take a list of file paths and search through |
| 228 | them with a given regex. Relevant messages will be printed to the console |
| 229 | and log. |
| 230 |     |
| 231 | Expected kwargs: file_paths, regex, message_text, message_box |
| 232 | """ |
| 233 |  # Start a list of dictionaries for files with interesting content. |
| 234 |  return_list  = [] |
| 235 |     |
| 236 |  # Open up each individual file and read the text into memory. |
| 237 |  for  file_name  in  kwargs['file_paths']: |
| 238 |  return_dict  = {} |
| 239 |     |
| 240 |  # Continue if we can't access the file. |
| 241 |  if  not  os.access(file_name, os.R_OK): |
| 242 |  continue |
| 243 |     |
| 244 |  file_object  =  open(file_name, 'r') |
| 245 |  file_text  =  file_object.read() |
| 246 |  # Use the regex we pass in to the function to look for vulns. |
| 247 |  found  =  re.findall(kwargs['regex'], file_text) |
| 248 |     |
| 249 |  # Save the file name and the interesting lines of text |
| 250 |  if  found: |
| 251 |  return_dict['file_name'] =  file_name |
| 252 |  return_dict['text'] =  found |
| 253 |  return_list.append(return_dict) |
| 254 |     |
| 255 |  # If the function is supplied with message info, print to console and log. |
| 256 |  # This function may be used instead as input to another function, so we |
| 257 |  # don't always want to print here. |
| 258 |  if  return_list  and  kwargs['message_text'] and  kwargs['message_box']: |
| 259 |  # Print to console and log the interesting file names and content. |
| 260 |  tee("") |
| 261 |  tee(kwargs['message_text'], box=kwargs['message_box']) |
| 262 |  for  item  in  return_list: |
| 263 |  tee(" {}:".format(item['file_name'])) |
| 264 |  for  text  in  item['text']: |
| 265 |  tee(" {}".format(text)) |
| 266 |  tee("") |
| 267 |     |
| 268 |  if  ARGS.debug: |
| 269 |  print("DEBUG, SEARCH RESULTS") |
| 270 |  for  item  in  return_list: |
| 271 |  print(item['file_name']) |
| 272 |  for  text  in  item['text']: |
| 273 |  print(" {}".format(text)) |
| 274 |     |
| 275 |  return  return_list |
| 276 |     |
| 277 |     |
| 278 | def  check_file_permissions(**kwargs): |
| 279 |  """Helper function to check permissions and symlink status |
| 280 |     |
| 281 | This function will take a list of file paths, resolve them to their |
| 282 | actual location (for symlinks), and determine if they are writeable |
| 283 | by the current user. Will also alert on broken symlinks and whether |
| 284 | the target directory for the broken link is writeable. |
| 285 |     |
| 286 | Expected kwargs: file_paths, files_message_text, dirs_message_text, |
| 287 | message_box |
| 288 | """ |
| 289 |  # Start deuplicated sets for interesting files and directories. |
| 290 |  writeable_files  =  set() |
| 291 |  writeable_dirs  =  set() |
| 292 |     |
| 293 |  for  file_name  in  kwargs['file_paths']: |
| 294 |     |
| 295 |  # Ignore known not-vulnerable targets |
| 296 |  if  file_name  in  NOT_VULN: |
| 297 |  continue |
| 298 |     |
| 299 |  # Is it a symlink? If so, get the real path and check permissions. |
| 300 |  # If it is broken, check permissions on the parent directory. |
| 301 |  if  os.path.islink(file_name): |
| 302 |  target  =  os.readlink(file_name) |
| 303 |     |
| 304 |  # Some symlinks use relative path names. Find these and prepend |
| 305 |  # the directory name so we can investigate properly. |
| 306 |  if  target[0] ==  '.': |
| 307 |  parent_dir  =  os.path.dirname(file_name) |
| 308 |  target  =  parent_dir  +  '/'  +  target |
| 309 |     |
| 310 |  if  os.path.exists(target) and  os.access(target, os.W_OK): |
| 311 |  writeable_files.add('{} -- symlink --> {}' |
| 312 | .format(file_name, target)) |
| 313 |  else: |
| 314 |  parent_dir  =  os.path.dirname(target) |
| 315 |  if  os.access(parent_dir, os.W_OK): |
| 316 |  writeable_dirs.add((file_name, target)) |
| 317 |     |
| 318 |  # OK, not a symlink. Just check permissions. |
| 319 |  else: |
| 320 |  if  os.access(file_name, os.W_OK): |
| 321 |  writeable_files.add(file_name) |
| 322 |     |
| 323 |  if  writeable_files: |
| 324 |  # Print to console and log the interesting findings. |
| 325 |  tee("") |
| 326 |  tee(kwargs['files_message_text'], box=kwargs['message_box']) |
| 327 |  for  item  in  writeable_files: |
| 328 |  tee(" {}".format(item)) |
| 329 |     |
| 330 |  if  writeable_dirs: |
| 331 |  # Print to console and log the interesting findings. |
| 332 |  tee("") |
| 333 |  tee(kwargs['dirs_message_text'], box=kwargs['message_box']) |
| 334 |  for  item  in  writeable_dirs: |
| 335 |  tee(" {} --> {}".format(item[0], item[1])) |
| 336 |     |
| 337 |  if  not  writeable_files  and  not  writeable_dirs: |
| 338 |  tee("") |
| 339 |  tee("No writeable targets. This is expected...", |
| 340 |  box='note') |
| 341 |     |
| 342 |     |
| 343 | def  check_command_permission(**kwargs): |
| 344 |  """Checks permissions on commands returned from inside files |
| 345 |     |
| 346 | Loops through a provided list of dictionaries with a file name and |
| 347 | commands found within. Checks to see if they are writeable or missing and |
| 348 | living in a writable directory. |
| 349 |     |
| 350 | Expected kwargs: file_paths, regex, message_text, message_box |
| 351 | """ |
| 352 |  # Start an empty list for the return of the writable files/commands |
| 353 |  return_list  = [] |
| 354 |     |
| 355 |  for  item  in  kwargs['commands']: |
| 356 |  return_dict  = {} |
| 357 |  return_dict['text'] = [] |
| 358 |     |
| 359 |  # The commands we have are long and may include parameters or even |
| 360 |  # multiple commands with pipes and ;. We try to split this all out |
| 361 |  # below. |
| 362 |  for  command  in  item['text']: |
| 363 |  command  =  re.sub(r'[\'"]', '', command) |
| 364 |  command  =  re.split(r'[ ;\\|]', command) |
| 365 |     |
| 366 |  # We now have a list of some commands and some parameters and |
| 367 |  # other garbage. Checking for os access will clean this up for us. |
| 368 |  # The lines below determine if we have write access to anything. |
| 369 |  # It also checks for the case where the target does not exist but |
| 370 |  # the parent directory is writeable. |
| 371 |  for  split_command  in  command: |
| 372 |  vuln  =  False |
| 373 |     |
| 374 |  # Ignore known not-vulnerable targets |
| 375 |  if  split_command  in  NOT_VULN: |
| 376 |  continue |
| 377 |     |
| 378 |  # Some systemd items will specicify a command with a path |
| 379 |  # relative to the calling item, particularly timer files. |
| 380 |  relative_path  =  os.path.dirname(item['file_name']) |
| 381 |     |
| 382 |  # First, check the obvious - is this a writable command? |
| 383 |  if  os.access(split_command, os.W_OK): |
| 384 |  vuln  =  True |
| 385 |     |
| 386 |  # What about if we assume it is a relative path? |
| 387 |  elif  os.access(relative_path  +  '/'  +  split_command, os.W_OK): |
| 388 |  vuln  =  True |
| 389 |     |
| 390 |  # Or maybe it doesn't exist at all, but is in a writeable |
| 391 |  # director? |
| 392 |  elif (os.access(os.path.dirname(split_command), os.W_OK) |
| 393 |  and  not  os.path.exists(split_command)): |
| 394 |  vuln  =  True |
| 395 |     |
| 396 |  # If so, pack it all up in a new dictionary which is used |
| 397 |  # below for output. |
| 398 |  if  vuln: |
| 399 |  return_dict['file_name'] =  item['file_name'] |
| 400 |  return_dict['text'].append(split_command) |
| 401 |  if  return_dict  not  in  return_list: |
| 402 |  return_list.append(return_dict) |
| 403 |     |
| 404 |  if  return_list  and  kwargs['message_text'] and  kwargs['message_box']: |
| 405 |  # Print to console and log the interesting file names and content. |
| 406 |  tee("") |
| 407 |  tee(kwargs['message_text'], box=kwargs['message_box']) |
| 408 |  for  item  in  return_list: |
| 409 |  tee(" {}:".format(item['file_name'])) |
| 410 |  for  text  in  item['text']: |
| 411 |  tee(" {}".format(text)) |
| 412 |  tee("") |
| 413 |     |
| 414 |     |
| 415 | ########################## Helper Functions Complete ########################## |
| 416 |     |
| 417 |     |
| 418 | ########################### Individual Checks Follow ########################## |
| 419 |     |
| 420 | # Note: naming a new function 'uptux_check_xxxx' will automatically |
| 421 | # include it in execution. These will trigger in the same order listed |
| 422 | # in the script. The docstring will be pulled and used in the console and |
| 423 | # log file, so keep it short (one line). |
| 424 |     |
| 425 | def  uptux_check_sysinfo(): |
| 426 |  """Gather basic OS information""" |
| 427 |  # Gather a few basics for the report. |
| 428 |  uname  =  os.uname() |
| 429 |  tee("Host: {}".format(uname[1])) |
| 430 |  tee("OS: {}, {}".format(uname[0], uname[3])) |
| 431 |  tee("Kernel: {}".format(uname[2])) |
| 432 |  tee("Current user: {} (UID {} GID {})".format(getpass.getuser(), |
| 433 |  os.getuid(), |
| 434 |  os.getgid())) |
| 435 |  tee("Member of following groups:\n {}".format(shell_exec('groups'))) |
| 436 |     |
| 437 |     |
| 438 | def  uptux_check_systemd_paths(): |
| 439 |  """Check if systemd PATH is writeable""" |
| 440 |  # Define the bash command. |
| 441 |  command  =  'systemctl show-environment' |
| 442 |  output  =  shell_exec(command) |
| 443 |     |
| 444 |  # Define the regex to find in the output. |
| 445 |  regex  =  re.compile(r'PATH=(.*$)') |
| 446 |     |
| 447 |  # Take the output from bash and split it into a list of paths. |
| 448 |  output  =  re.findall(regex, output) |
| 449 |     |
| 450 |  # This command may fail in some environments, only proceed if we have |
| 451 |  # a good match. |
| 452 |  if  output: |
| 453 |  output  =  output[0].split(':') |
| 454 |     |
| 455 |  # Check each path - if it is writable, add it to a list. |
| 456 |  writeable_paths  = [] |
| 457 |  for  item  in  output: |
| 458 |  if  os.access(item, os.W_OK): |
| 459 |  writeable_paths.append(item) |
| 460 |  else: |
| 461 |  writeable_paths  =  False |
| 462 |     |
| 463 |  # Write the status to the console and log. |
| 464 |  if  writeable_paths: |
| 465 |  tee("The following systemd paths are writeable. THIS IS ODD!\n" |
| 466 |  "See if you can combine this with a relative path Exec statement" |
| 467 |  " for privesc:", |
| 468 |  box='vuln') |
| 469 |  for  path  in  writeable_paths: |
| 470 |  tee(" {}".format(path)) |
| 471 |  global  SYSTEMD_PATH_WRITABLE |
| 472 |  SYSTEMD_PATH_WRITABLE  =  True |
| 473 |  else: |
| 474 |  tee("No systemd paths are writeable. This is expected...", |
| 475 |  box='note') |
| 476 |     |
| 477 |     |
| 478 | def  uptux_check_services(): |
| 479 |  """Inspect systemd service unit files""" |
| 480 |  # Define known Linux folders for storing service unit definitions |
| 481 |  units  =  set() |
| 482 |  mask  =  '*.service' |
| 483 |  units  =  find_system_files(known_dirs=SYSTEMD_DIRS, |
| 484 |  search_mask=mask) |
| 485 |     |
| 486 |  tee("Found {} service units to analyse...\n".format(len(units)), |
| 487 |  box='ok') |
| 488 |     |
| 489 |  # Test for write access to any service files. |
| 490 |  # Will resolve symlinks to their target and also check for broken links. |
| 491 |  text  =  'Found writeable service unit files:' |
| 492 |  text2  =  'Found writeable directories referred to by broken symlinks' |
| 493 |  box  =  'vuln' |
| 494 |  tee("") |
| 495 |  tee("Checking permissions on service unit files...", |
| 496 |  box='ok') |
| 497 |  check_file_permissions(file_paths=units, |
| 498 |  files_message_text=text, |
| 499 |  dirs_message_text=text2, |
| 500 |  message_box=box) |
| 501 |     |
| 502 |  # Only check relative paths if we can abuse them |
| 503 |  if  SYSTEMD_PATH_WRITABLE: |
| 504 |  # Look for relative calls to binaries. |
| 505 |  # Example: ExecStart=somfolder/somebinary |
| 506 |  regex  =  re.compile(r'^Exec(?:Start\|Stop\|Reload)=' |
| 507 |  r'(?:@[^/]'  # special exec |
| 508 |  r'\|-[^/]'  # special exec |
| 509 |  r'\|\+[^/]'  # special exec |
| 510 |  r'\|![^/]'  # special exec |
| 511 |  r'\|!![^/]'  # special exec |
| 512 |  r'\|)'  # or maybe no special exec |
| 513 |  r'[^/@\+!-]'  # not abs path or special exec |
| 514 |  r'.*', # rest of line |
| 515 |  re.MULTILINE) |
| 516 |  text  = ('Possible relative path in an Exec statement.\n' |
| 517 |  'Unless you have writeable systemd paths, you won\'t be able to' |
| 518 |  ' exploit this:') |
| 519 |  box  =  'sus' |
| 520 |  tee("") |
| 521 |  tee("Checking for relative paths in service unit files [check 1]...", |
| 522 |  box='ok') |
| 523 |  regex_vuln_search(file_paths=units, |
| 524 |  regex=regex, |
| 525 |  message_text=text, |
| 526 |  message_box=box) |
| 527 |     |
| 528 |  # Look for relative calls to binaries but invoked by an interpreter. |
| 529 |  # Example: ExecStart=/bin/sh -c 'somefolder/somebinary' |
| 530 |  regex  =  re.compile(r'^Exec(?:Start\|Stop\|Reload)=' |
| 531 |  r'(?:@[^/]'  # special exec |
| 532 |  r'\|-[^/]'  # special exec |
| 533 |  r'\|\+[^/]'  # special exec |
| 534 |  r'\|![^/]'  # special exec |
| 535 |  r'\|!![^/]'  # special exec |
| 536 |  r'\|)'  # or maybe no special exec |
| 537 |  r'.*?(?:/bin/sh\|/bin/bash) '  # interpreter |
| 538 |  r'(?:[\'"]\|)'  # might have quotes |
| 539 |  r'(?:-[a-z]+\|)'# might have params |
| 540 |  r'(?:[ ]+\|)'  # might have more spaces now |
| 541 |  r'[^/-]'  # not abs path or param |
| 542 |  r'.*', # rest of line |
| 543 |  re.MULTILINE) |
| 544 |  text  = ('Possible relative path invoked with an interpreter in an' |
| 545 |  ' Exec statement.\n' |
| 546 |  'Unless you have writable systemd paths, you won\'t be able to' |
| 547 |  ' exploit this:') |
| 548 |  box  =  'sus' |
| 549 |  tee("") |
| 550 |  tee("Checking for relative paths in service unit files [check 2]...", |
| 551 |  box='ok') |
| 552 |  regex_vuln_search(file_paths=units, |
| 553 |  regex=regex, |
| 554 |  message_text=text, |
| 555 |  message_box=box) |
| 556 |     |
| 557 |  # Check for write access to any commands invoked by Exec statements. |
| 558 |  # Thhs regex below is used to extract command lines. |
| 559 |  regex  =  re.compile(r'^Exec.*?=[!@+-]*(.*?$)', |
| 560 |  re.MULTILINE) |
| 561 |  # We don't pass message info to this function as we need to perform more |
| 562 |  # processing on the output to determine what is writeable. |
| 563 |  tee("") |
| 564 |  tee("Checking for write access to commands referenced in service files...", |
| 565 |  box='ok') |
| 566 |  service_commands  =  regex_vuln_search(file_paths=units, |
| 567 |  regex=regex, |
| 568 |  message_text='', |
| 569 |  message_box='') |
| 570 |     |
| 571 |  # Another helper function to take the extracted commands and check for |
| 572 |  # write permissions. |
| 573 |  text  =  'You have write access to commands referred to in service files:' |
| 574 |  box  =  'vuln' |
| 575 |  check_command_permission(commands=service_commands, |
| 576 |  message_text=text, |
| 577 |  message_box=box) |
| 578 |     |
| 579 |     |
| 580 | def  uptux_check_timer_units(): |
| 581 |  """Inspect systemd timer unit files""" |
| 582 |  units  =  set() |
| 583 |  mask  =  '*.timer' |
| 584 |  units  =  find_system_files(known_dirs=SYSTEMD_DIRS, |
| 585 |  search_mask=mask) |
| 586 |     |
| 587 |  tee("Found {} timer units to analyse...\n".format(len(units)), |
| 588 |  box='ok') |
| 589 |     |
| 590 |  # Test for write access to any timer files. |
| 591 |  # Will resolve symlinks to their target and also check for broken links. |
| 592 |  text  =  'Found writeable timer unit files:' |
| 593 |  text2  =  'Found writeable directories referred to by broken symlinks' |
| 594 |  box  =  'vuln' |
| 595 |  tee("") |
| 596 |  tee("Checking permissions on timer unit files...", |
| 597 |  box='ok') |
| 598 |  check_file_permissions(file_paths=units, |
| 599 |  files_message_text=text, |
| 600 |  dirs_message_text=text2, |
| 601 |  message_box=box) |
| 602 |     |
| 603 |  # Timers may reference systemd services, which are already being checked. |
| 604 |  # But they may reference a specific script (often a '.target' file of the |
| 605 |  # same name. Check to see if the action called is writable. |
| 606 |  # The regex below is used to extract these targets. |
| 607 |  regex  =  re.compile(r'^Unit=*(.*?$)', |
| 608 |  re.MULTILINE) |
| 609 |     |
| 610 |  # We don't pass message info to this function as we need to perform more |
| 611 |  # processing on the output to determine what is writeable. |
| 612 |  tee("") |
| 613 |  tee("Checking for write access to commands referenced in timer files...", |
| 614 |  box='ok') |
| 615 |  timer_commands  =  regex_vuln_search(file_paths=units, |
| 616 |  regex=regex, |
| 617 |  message_text='', |
| 618 |  message_box='') |
| 619 |     |
| 620 |  # Another helper function to take the extracted commands and check for |
| 621 |  # write permissions. |
| 622 |  text  =  'You have write access to commands referred to in timer files:' |
| 623 |  box  =  'vuln' |
| 624 |  check_command_permission(commands=timer_commands, |
| 625 |  message_text=text, |
| 626 |  message_box=box) |
| 627 |     |
| 628 |     |
| 629 | def  uptux_check_socket_units(): |
| 630 |  """Inspect systemd socket unit files""" |
| 631 |  units  =  set() |
| 632 |  mask  =  '*.socket' |
| 633 |  units  =  find_system_files(known_dirs=SYSTEMD_DIRS, |
| 634 |  search_mask=mask) |
| 635 |     |
| 636 |  tee("Found {} socket units to analyse...\n".format(len(units)), |
| 637 |  box='ok') |
| 638 |     |
| 639 |  # Test for write access to any socket files. |
| 640 |  # Will resolve symlinks to their target and also check for broken links. |
| 641 |  text  =  'Found writeable socket unit files:' |
| 642 |  text2  =  'Found writeable directories referred to by broken symlinks' |
| 643 |  box  =  'vuln' |
| 644 |  tee("") |
| 645 |  tee("Checking permissions on socket unit files...", |
| 646 |  box='ok') |
| 647 |  check_file_permissions(file_paths=units, |
| 648 |  files_message_text=text, |
| 649 |  dirs_message_text=text2, |
| 650 |  message_box=box) |
| 651 |     |
| 652 |  # Check for write access to any socket files created by a service. |
| 653 |  # This can be interesting - I've seen a systemd service run a REST |
| 654 |  # API on a AF_UNIX socket. This would be missed by normal privesc |
| 655 |  # checks. |
| 656 |  # Thhs regex below is used to extract command lines. |
| 657 |  regex  =  re.compile(r'^Listen.*?=[!@+-]*(.*?$)', |
| 658 |  re.MULTILINE) |
| 659 |     |
| 660 |  # We don't pass message info to this function as we need to perform more |
| 661 |  # processing on the output to determine what is writeable. |
| 662 |  tee("") |
| 663 |  tee("Checking for write access to AF_UNIX sockets...", |
| 664 |  box='ok') |
| 665 |  socket_files  =  regex_vuln_search(file_paths=units, |
| 666 |  regex=regex, |
| 667 |  message_text='', |
| 668 |  message_box='') |
| 669 |     |
| 670 |  # Another helper function to take the extracted commands and check for |
| 671 |  # write permissions. |
| 672 |  text  = ('You have write access to AF_UNIX socket files invoked by a' |
| 673 |  ' systemd service.\n' |
| 674 |  'This could be interesting. \n' |
| 675 |  'You can attach to these files to look for an exploitable API.') |
| 676 |  box  =  'sus' |
| 677 |  check_command_permission(commands=socket_files, |
| 678 |  message_text=text, |
| 679 |  message_box=box) |
| 680 |     |
| 681 |     |
| 682 | def  uptux_check_socket_apis(): |
| 683 |  """Look for web servers on UNIX domain sockets""" |
| 684 |  # Use Linux ss tool to find sockets in listening state |
| 685 |  command  =  'ss -xlp -H state listening' |
| 686 |  output  =  shell_exec(command) |
| 687 |     |
| 688 |  # We get Unicode back from the above command, let's fix |
| 689 |  output  =  str(output) |
| 690 |     |
| 691 |  root_sockets  = [] |
| 692 |  socket_replies  = {} |
| 693 |     |
| 694 |  # We want to grab all the strings that look like socket paths |
| 695 |  sockets  =  re.findall(r' (/.*?) ', output) |
| 696 |  abstract_sockets  =  re.findall(r' (@.*?) ', output) |
| 697 |     |
| 698 |  for  socket_path  in  sockets: |
| 699 |  # For now, we are only interested in sockets owned by root |
| 700 |  if  os.path.exists(socket_path) and  os.stat(socket_path).st_uid  ==  0: |
| 701 |  root_sockets.append(socket_path) |
| 702 |     |
| 703 |  tee("Trying to connect to {} unix sockets owned by uid 0..." |
| 704 | .format(len(root_sockets)), box='ok') |
| 705 |  tee("") |
| 706 |     |
| 707 |  # Cycle through each and try to send a raw HTTP GET |
| 708 |  for  socket_target  in  root_sockets: |
| 709 |     |
| 710 |  # Define a raw HTTP GET request |
| 711 |  http_get  = ('GET / HTTP/1.1\n' |
| 712 |  'Host: localhost\n' |
| 713 |  '\n\n') |
| 714 |     |
| 715 |  # Try to interact with the socket like a web API |
| 716 |  client  =  socket.socket(socket.AF_UNIX, socket.SOCK_STREAM) |
| 717 |  client.settimeout(5) |
| 718 |  try: |
| 719 |  client.connect(socket_target) |
| 720 |  client.sendall(http_get.encode()) |
| 721 |     |
| 722 |  reply  =  client.recv(8192).decode() |
| 723 |     |
| 724 |  # If we get a reply to this, we assume it is an API |
| 725 |  if  reply: |
| 726 |  socket_replies[socket_target] =  reply |
| 727 |     |
| 728 |  except (socket.error, UnicodeDecodeError): |
| 729 |  continue |
| 730 |     |
| 731 |  # If we have some replies, print to console |
| 732 |  # Hack-ish string replacement to get a nice indent |
| 733 |  if  socket_replies: |
| 734 |  tee("The following root-owned sockets replied as follows", |
| 735 |  box='sus') |
| 736 |  for  socket_path  in  socket_replies: |
| 737 |  tee(" "  +  socket_path  +  ":") |
| 738 |  tee(" "  +  socket_replies[socket_path] |
| 739 | .replace('\n', '\n ')) |
| 740 |  tee("") |
| 741 |     |
| 742 |     |
| 743 | def  uptux_check_dbus_issues(): |
| 744 |  """Inspect D-Bus configuration items""" |
| 745 |  units  =  set() |
| 746 |  mask  =  '*.conf' |
| 747 |  units  =  find_system_files(known_dirs=DBUS_DIRS, |
| 748 |  search_mask=mask) |
| 749 |     |
| 750 |  tee("Found {} D-Bus conf files to analyse...\n".format(len(units)), |
| 751 |  box='ok') |
| 752 |     |
| 753 |  # Test for write access to any files. |
| 754 |  # Will resolve symlinks to their target and also check for broken links. |
| 755 |  text  =  'Found writeable D-Bus conf files:' |
| 756 |  text2  =  'Found writeable directories referred to by broken symlinks' |
| 757 |  box  =  'vuln' |
| 758 |  tee("") |
| 759 |  tee("Checking permissions on D-Bus conf files...", |
| 760 |  box='ok') |
| 761 |  check_file_permissions(file_paths=units, |
| 762 |  files_message_text=text, |
| 763 |  dirs_message_text=text2, |
| 764 |  message_box=box) |
| 765 |     |
| 766 |  # Checking for overly permission policies in D-Bus configuration files. |
| 767 |  # For example, normally "policy" is defined as a username. When defined in |
| 768 |  # an XML tag on its own, it applies to everyone. |
| 769 |  tee("") |
| 770 |  tee("Checking for overly permissive D-Bus configuration rules...", |
| 771 |  box='ok') |
| 772 |     |
| 773 |  regex  =  re.compile(r'<policy>.*?</policy>', |
| 774 |  re.MULTILINE\|re.DOTALL) |
| 775 |     |
| 776 |  text  = ('These D-Bus policies may be overly permissive as they do not' |
| 777 |  ' specify a user or group.') |
| 778 |  box  =  'sus' |
| 779 |  regex_vuln_search(file_paths=units, |
| 780 |  regex=regex, |
| 781 |  message_text=text, |
| 782 |  message_box=box) |
| 783 |     |
| 784 | ########################## Individual Checks Complete ######################### |
| 785 |     |
| 786 | def  main(): |
| 787 |  """Main function""" |
| 788 |  print(BANNER) |
| 789 |     |
| 790 |  # Dynamically build list of checks to execute. |
| 791 |  uptux_checks  =  build_checks_list() |
| 792 |     |
| 793 |  # Use the handler to execute each check. |
| 794 |  for  check  in  uptux_checks: |
| 795 |  check_name  =  check.__name__ |
| 796 |  check_desc  =  check.__doc__ |
| 797 |  check_handler(check, check_name, check_desc) |
| 798 |     |
| 799 |  # Good luck! |
| 800 |  tee("") |
| 801 |  tee("All done, good luck!", box='note') |
| 802 |     |
| 803 | if  __name__  ==  "__main__": |
| 804 |  main() |