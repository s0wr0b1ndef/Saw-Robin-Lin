## Redteamer's cheatsheet
Who am i ? >>> Robin(Blueteamer) 

### Enumeration 

Nmap - initial one, a full one and an UDP one, all of them in parallel

```markdown
nmap -sV -O --top-ports 50 --open -oA nmap/initial <ip or cidr>
nmap -sC -sV -O --open -p- -oA nmap/full <ip or cidr>
nmap -sU -p- -oA nmap/udp <ip or cidr>

--top-ports only scan the N most common ports
--open only show open ports
-sC use the default scripts
-sV detect versions
-O detect Operating Systems
-p- scan all the ports
-oA save the output in normal format, grepable and xml
-sU scan UDP ports

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/s0wr0b1ndef/Notes/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
