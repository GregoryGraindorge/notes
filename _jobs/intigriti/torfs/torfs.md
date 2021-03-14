# Intigriti

## Tier 2

- www.winkels.torfs.be
- www.schoenentorfs.be
- www.schoenentorfs.nl
- www.torfs.be
- www.torfs.nl

## Tier 3

### www.sterkinjeschoenen.be

- Apache `2.4.25`
	- Actual version `2.4.46`
- PrettyPhoto
- Debian

- [Sellingent](https://www.selligent.com/)  

		https://www.biendansseschaussures.be/fr/histoire-texte/?selligent_id=nPTABPUywB6ACtSaMkHNavQoNY3FLbUZzfR5_hS1NC8R60WAgbTiEi%2BW7ukTYszWtgg3aGVJBb9nn0

- Possible Directory Traversal 

		https://www.biendansseschaussures.be/swfiles/files/download.php?myfile=

| files | responses |
| :---- | :-------- |
| index.php | invalid file | 
| index.html | OK | 
cape Unicode CharactersNormalise Unicode index.html5 | <b>404 File not found!</b> |

Every extensions are accepted for the file except `PHP`.

| urd code to bypass filter | response |
| :------------------------ | :------- |
| %252f, %C0%AF, %f0%80%80%af, %e0%80%af | file not found |
| %C0%2F | invalid file | 

- www.torfssuppliers.be
- www.samenfittorfs.be
