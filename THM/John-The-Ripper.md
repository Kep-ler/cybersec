There are multiple ways to use John the Ripper to crack simple hashes. We’ll walk through a few before moving on to cracking some ourselves.

John Basic Syntax

The basic syntax of John the Ripper commands is as follows. We will cover the specific options and modifiers used as we use them.

john [options] [file path]

john: Invokes the John the Ripper program
[options]: Specifies the options you want to use
[file path]: The file containing the hash you’re trying to crack; if it’s in the same directory, you won’t need to name a path, just the file.
Automatic Cracking

John has built-in features to detect what type of hash it’s being given and to select appropriate rules and formats to crack it for you; this isn’t always the best idea as it can be unreliable, but if you can’t identify what hash type you’re working with and want to try cracking it, it can be a good option! To do this, we use the following syntax:

john --wordlist=[path to wordlist] [path to file]

--wordlist=: Specifies using wordlist mode, reading from the file that you supply in the provided path
[path to wordlist]: The path to the wordlist you’re using, as described in the previous task
Example Usage:

john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

Identifying Hashes

Sometimes, John won’t play nicely with automatically recognising and loading hashes, but that’s okay! We can use other tools to identify the hash and then set John to a specific format. There are multiple ways to do this, such as using an online hash identifier like this site. I like to use a tool called hash-identifier, a Python tool that is super easy to use and will tell you what different types of hashes the one you enter is likely to be, giving you more options if the first one fails.

To use hash-identifier, you can use wget or curl to download the Python file hash-id.py from its GitLab page. Then, launch it with python3 hash-id.py and enter the hash you’re trying to identify. It will give you a list of the most probable formats. These two steps are shown in the terminal below.

Terminal
user@TryHackMe$ wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
$ python3 hash-id.py
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 2e728dd31fb5949bc39cac5a9f066498

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))
Format-Specific Cracking

Once you have identified the hash that you’re dealing with, you can tell John to use it while cracking the provided hash using the following syntax:

john --format=[format] --wordlist=[path to wordlist] [path to file]

--format=: This is the flag to tell John that you’re giving it a hash of a specific format and to use the following format to crack it
[format]: The format that the hash is in
Example Usage:

john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

A Note on Formats:

When you tell John to use formats, if you’re dealing with a standard hash type, e.g. md5 as in the example above, you have to prefix it with raw- to tell John you’re just dealing with a standard hash type, though this doesn’t always apply. To check if you need to add the prefix or not, you can list all of John’s formats using john --list=formats and either check manually or grep for your hash type using something like john --list=formats | grep -iF "md5".

Practical

Now that you know the syntax, modifiers, and methods for cracking basic hashes, try it yourself! The files are located in ~/John-the-Ripper-The-Basics/Task04/ on the attached virtual machine.

NTHash / NTLM

NThash is the hash format modern Windows operating system machines use to store user and service passwords. It’s also commonly referred to as NTLM, which references the previous version of Windows format for hashing passwords known as LM, thus NT/LM.

A bit of history: the NT designation for Windows products originally meant New Technology. It was used starting with Windows NT to denote products not built from the MS-DOS Operating System. Eventually, the “NT” line became the standard Operating System type to be released by Microsoft, and the name was dropped, but it still lives on in the names of some Microsoft technologies.

In Windows, SAM (Security Account Manager) is used to store user account information, including usernames and hashed passwords. You can acquire NTHash/NTLM hashes by dumping the SAM database on a Windows machine, using a tool like Mimikatz, or using the Active Directory database: NTDS.dit. You may not have to crack the hash to continue privilege escalation, as you can often conduct a “pass the hash” attack instead, but sometimes, hash cracking is a viable option if there is a weak password policy.

Practical

Now that you know the theory behind it, see if you can use the techniques we practised in the last task and the knowledge of what type of hash this is to crack the ntlm.txt file! The file is located in ~/John-the-Ripper-The-Basics/Task05/.

What are Custom Rules?

As we explored what John can do in Single Crack Mode, you may have some ideas about some good mangling patterns or what patterns your passwords often use that could be replicated with a particular mangling pattern. The good news is that you can define your rules, which John will use to create passwords dynamically. The ability to define such rules is beneficial when you know more information about the password structure of whatever your target is.

Common Custom Rules

Many organisations will require a certain level of password complexity to try and combat dictionary attacks. In other words, when creating a new account or changing your password, if you attempt a password like polopassword, it will most likely not work. The reason would be the enforced password complexity. As a result, you may receive a prompt telling you that passwords have to contain at least one character from each of the following:

Lowercase letter
Uppercase letter
Number
Symbol
Password complexity is good! However, we can exploit the fact that most users will be predictable in the location of these symbols. For the above criteria, many users will use something like the following:

Polopassword1!

Consider the password with a capital letter first and a number followed by a symbol at the end. This familiar pattern of the password, appended and prepended by modifiers (such as capital letters or symbols), is a memorable pattern that people use and reuse when creating passwords. This pattern can let us exploit password complexity predictability.

Now, this does meet the password complexity requirements; however, as attackers, we can exploit the fact that we know the likely position of these added elements to create dynamic passwords from our wordlists.

How to create Custom Rules

Custom rules are defined in the john.conf file. This file can be found in /opt/john/john.conf on the TryHackMe Attackbox. It is usually located in /etc/john/john.conf if you have installed John using a package manager or built from source with make.

Let’s go over the syntax of these custom rules, using the example above as our target pattern. Note that you can define a massive level of granular control in these rules. I suggest looking at the wiki here to get a full view of the modifiers you can use and more examples of rule implementation.

The first line:

[List.Rules:THMRules] is used to define the name of your rule; this is what you will use to call your custom rule a John argument.

We then use a regex style pattern match to define where the word will be modified; again, we will only cover the primary and most common modifiers here:

Az: Takes the word and appends it with the characters you define
A0: Takes the word and prepends it with the characters you define
c: Capitalises the character positionally
These can be used in combination to define where and what in the word you want to modify.

Lastly, we must define what characters should be appended, prepended or otherwise included. We do this by adding character sets in square brackets [ ] where they should be used. These follow the modifier patterns inside double quotes " ". Here are some common examples:

[0-9]: Will include numbers 0-9
[0]: Will include only the number 0
[A-z]: Will include both upper and lowercase
[A-Z]: Will include only uppercase letters
[a-z]: Will include only lowercase letters
Please note that:

[a]: Will include only a
[!£$%@]: Will include the symbols !, £, $, %, and @
Putting this all together, to generate a wordlist from the rules that would match the example password Polopassword1! (assuming the word polopassword was in our wordlist), we would create a rule entry that looks like this:

[List.Rules:PoloPassword]

cAz"[0-9] [!£$%@]"

Utilises the following:

c: Capitalises the first letter
Az: Appends to the end of the word
[0-9]: A number in the range 0-9
[!£$%@]: The password is followed by one of these symbols
Using Custom Rules

We could then call this custom rule a John argument using the  --rule=PoloPassword flag.

As a full command: john --wordlist=[path to wordlist] --rule=PoloPassword [path to file]

As a note, I find it helpful to talk out the patterns if you’re writing a rule; as shown above, the same applies to writing RegEx patterns.

Jumbo John already has an extensive list of custom rules containing modifiers for use in almost all cases. If you get stuck, try looking at those rules [around line 678] if your syntax isn’t working correctly.

Now, it’s time for you to have a go!

Cracking Password Protected Zip Files

Yes! You read that right. We can use John to crack the password on password-protected Zip files. Again, we’ll use a separate part of the John suite of tools to convert the Zip file into a format that John will understand, but we’ll use the syntax you’re already familiar with for all intents and purposes.

Zip2John

Similarly to the unshadow tool we used previously, we will use the zip2john tool to convert the Zip file into a hash format that John can understand and hopefully crack. The primary usage is like this:

zip2john [options] [zip file] > [output file]

[options]: Allows you to pass specific checksum options to zip2john; this shouldn’t often be necessary
[zip file]: The path to the Zip file you wish to get the hash of
>: This redirects the output from this command to another file
[output file]: This is the file that will store the output


zip2john zipfile.zip > zip_hash.txt

Cracking

We’re then able to take the file we output from zip2john in our example use case, zip_hash.txt, and, as we did with unshadow, feed it directly into John as we have made the input specifically for it.

john --wordlist=/usr/share/wordlists/rockyou.txt zip_hash.txt


<img width="1680" height="1050" alt="JTR2" src="https://github.com/user-attachments/assets/1d90098a-f9db-4361-8769-0bcc466c46a2" />
<img width="1535" height="865" alt="jtr" src="https://github.com/user-attachments/assets/db98aa9c-59ce-4c8e-b81d-be64a8c63649" />

Cracking an SSH Private Key Password Using John the Ripper
In my latest hands-on lab, I explored cracking password-protected SSH private keys using John the Ripper. SSH key-based authentication is a common method for secure login, but private keys are often password-protected. This exercise demonstrated how to audit and recover such passwords in a controlled environment.
Workflow
Step 1: Convert the SSH Key to a Hash
John cannot read an encrypted private key directly. Using ssh2john, the id_rsa file was converted into a hash format suitable for John:

cd ~/John-the-Ripper-The-Basics/Task11/
python3 /opt/john/ssh2john.py id_rsa > id_rsa_hash.txt
This produces id_rsa_hash.txt, which contains the password hash.
Step 2: Perform a Dictionary Attack
The hash was then attacked using a wordlist:

john --wordlist=/usr/share/wordlists/rockyou.txt id_rsa_hash.txt
John systematically tried passwords from the wordlist until the correct password was found.
Step 3: Reveal the Password

john --show id_rsa_hash.txt
This command displayed the recovered SSH private key password, completing the exercise.
Key Takeaways
How private key encryption works in SSH and why strong passwords are crucial
The practical use of conversion tools like ssh2john in password auditing
Applying wordlist-based attacks in ethical hacking scenarios
Reinforced the workflow of hash extraction, attack, and verification
Tools Used
Kali Linux
John the Ripper
ssh2john.py
rockyou.txt wordlist
This lab strengthened my practical understanding of offensive security techniques and reinforced the importance of secure key management in real-world environments.

<img width="1535" height="865" alt="jtr" src="https://github.com/user-attachments/assets/04bf8851-a33a-427c-bc7d-bffb0d8bf7dd" />


