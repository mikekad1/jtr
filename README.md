<h1>Cracking with John The Ripper</h1>

In this project I will be using John the Ripper to crack password on my Linux machine using a pre-defined wordlist. 
There are plenty of choices when it comes to wordlists online, but for my project I decided to go with something simple and known, like the list provided by danielmiessler:
![Screenshot 2023-10-02 115343](https://github.com/mikekad1/jtr/assets/62948569/1a9db709-7e7b-49ab-a43c-4aa0df38d70e)

After successfully downloading the wordlist it is time to fire up my Linux machine and change the password to a dictionary word:
![Screenshot 2023-10-02 120223](https://github.com/mikekad1/jtr/assets/62948569/82b01543-bd17-461f-a604-8f74ad8c341d)

Password set to computermonkey a combination of words from wordlist. Method which I learned from by making a bit of research <a href="https://www.openwall.com/lists/john-users/2008/10/17/2">here</a>.
Before we can crack a password, we need to make sure there is one to crack. In this case, I copied shadow file to the Desktop:
![Screenshot 2023-10-02 121308](https://github.com/mikekad1/jtr/assets/62948569/fa063f55-b742-4808-aba0-9bf56de044e2)

I was having difficulty using a wordlist on a shadow file (silly me), until I found <a href="https://www.freecodecamp.org/news/crack-passwords-using-john-the-ripper-pentesting-tutorial/#:~:text=The%20unshadow%20command%20combines%20the,by%20John%20to%20crack%20passwords.&text=This%20command%20will%20combine%20the,db%20file.">this</a> article, showing me how to use unshadow to create file for john to crack.
Another difficulty that I encountered while attempting to crack the password is the need to specify <a href="https://security.stackexchange.com/questions/252665/does-john-the-ripper-not-support-yescrypt">format</a> to use.
![Screenshot 2023-10-03 135552](https://github.com/mikekad1/jtr/assets/62948569/3cbb8330-2220-4891-8842-8a10a807a111)

Finally, after some trial and error the password was cracked. And verified.
![Screenshot 2023-10-03 135705](https://github.com/mikekad1/jtr/assets/62948569/be2709a0-7afa-4ff5-838c-644834c42386)

While making the research on combining the words from wordlist, I came across the <a href="https://www.openwall.com/lists/john-users/2008/10/17/2">Openwall</a> webpage, which showed various methods of word permutations. To achieve these, Openwall suggests creating a Rule in john.config that would give instructions for JtR to use specific algorithm while reading from wordlist.
This method would make sense in a smaller, compact list. For instance. If my wordlist contained following:
- computer
- monkey
- password
Than it would logically mean that, I’d have to make an entry in the john.config to be:
[List.Rules:Wordlist]
$c$o$m$p$u$t$e$r
$m$o$n$k$e$y
$p$a$s$s$w$o$r$d

Where “$” is the append character. The logic behind it is that JtR can only append second word from outside of wordlist. 
Another way of doing it, would be utilizing Perl code with john. Openwall provides it as:
#!/usr/bin/perl

while (<>) {
	chop;
	$w[$#w + 1] = $_;
}

foreach $a (@w) {
	foreach $b (@w) {
		print "$a$b\n";
		print "$a $b\n";
	}
}

As described <a href="https://www.akimbocore.com/article/custom-rules-for-john-the-ripper/">in this blog</a>, JtR is highly customizable and there is a large variety of rules which can be incorporated, as long as one knows and understands how the character rules work.


