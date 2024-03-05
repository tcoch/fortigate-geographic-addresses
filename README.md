<h1>FortiGate - Geographic Addresses</h1>
<hline/>
  
Creating geographic addresses on FortiGate can be long if you need to declare a lot of countries. This repository aims to provide you the full list of countries supported by your FortiGate.

<h2>The full list</h2>

Full list is available at [cli.txt](cli.txt). You can copy / paste this directly to your equipment using SSH / console access.

<h2>How to create this list</h2>

I get it, you can be suspicious. Are there any errors in this list ? Is it up to date ? <strong>Can I trust it ??</strong>

If you prefer, you can re-create it, following these steps :

<h3>Retrieve the list of supported country codes</h3>

1. Connect to your FortiGate using console / SSH. Type in `config firewall address`. Then, create a new dummy object using `edit "dummy object"`.
2. Type in command `set type geography`. Then, type `set country ?` to see the full list of country code supported by your equipment.
3. Copy/paste this list

<h3>Parse the list and create the CLI commands</h3>

Paste in the previous list in Notepad++ (or some other tool, but then you might have to adapt the following commands).

Use replace utility, enabling regular expressions, and replace the following expressions :

<ins>1. Create the "set country" command</ins>

Find :
```
(.*)\s{4}(.*)
```
<i>Meaning : Something, then 4 spaces, then something else</i>

And replace it with :
```
`$2\n    set country "$1"`
```
<i>Meaning : Insert the content of the second (.\*), go to the next line, insert 4 spaces, insert 'set country ', and insert the content of the first (.\*)</i>

<ins>2. Create the "edit" command</ins>

Find :
```
^(?!\s{4})(.*)$
```
<i>Meaning : Something, that doesn't start with 4 spaces</i>

And replace it with :
```
$edit "$1"
```
<i>Meaning : Insert 'edit "', then the string you found, then close the quotation mark</i>

<ins>3. Create the "set type geography" command</ins>

Find :
```
edit "(.*)"
```
<i>Meaning : Something, that doesn't start with 4 spaces</i>

And replace it with :
```
$edit "edit "$1"\n    set type geography"
```
<i>Meaning : Keep the 'edit' string, add a new line, add four spaces, and set insert 'set type geography'</i>

<ins>4. Add the "next" command</ins>

Find :
```
set country "(.*)"
```
<i>Meaning : Something, that doesn't start with 4 spaces</i>

And replace it with :
```
$edit "set country "$1"\nnext
```
<i>Meaning : Keep the 'edit' string, add a new line, add four spaces, and set insert 'set type geography'</i>
