
<h1> pointers to get next line project </h1>
<hr>
In this readme file i'll be talking about the code of my <b>get next line</b> project so read carefully ^^.

But, before we dive into the code let's discuss :
	<h2>the big picture of this project.</h2>
<div>
This project is part of the 42 cursus commun core. (GET_NEXT_LINE)
	<h5>NOTICE :</h5>if you're also working on this project please check if the subject is the same as this one to avoid any problemes.
        even if your not a 42 student you can do these project to undrestand new concepts.
        and happy coding (iel-bakk).
</div>
<div>
<ol>
	<p>I'll start talking about what get next line would add to your knowledge see, the get next line project aims to teach you :</p>
	<li> how to read from a file using a file descriptor.</li>
	<li> what are static variables.</li>
	<li> how to avoid leaks.</li>
</ol>
</div>

These are what i think the things that you will need to know in order to get this project done.
The emplementation of these tools (the three thing i mentioned above) in your code will be the judge of your codes quality.

As i said before the goal of this project is to be able to read a file line by line with the '\n' included in each line.
So as a first step we need to get our hands on The file descriptor wich will be our key to access the file in order to read from it, this file discreptor is something that we get by calling the system call (OPEN).
After getting our file descriptor with the reading permission we can read form our file, but while reading ther are some rules that we must not break and these rules are :
<ul>
        <li><b>YOU CAN READ ONLY A LIMITED NUMBER OF BYTES EACH TIME YOU CALL READ.</b></li>
        <li><b>THE NUMBER OF BYTES YOUR ALLOWED TO READ IS DEFIEND BY THE USER (BUFFER_SIZE).</b></li>
        <li><b>EACH TIME YOU CALL YOUR GET NEXT LINE FUNCTION YOU MUST RETURN A SIGNLE LINE WITH THE NEW LINE INCLUDED.</b></li>
        <li><b>CALLING YOUR GET NEXT LINE FUNCTION IN A LOOP SHOULD PRINT THE WHOLE FILE CONTENT.</b></li>
        <li><b>UR CODE MUST NOT HAVE ANY LEAKS.</b></li>
</ul>
these are the rules concerning the output of our function (mandatory part ofcours).

<h2>Now, we can dive thru our code.</h2>
<p>
So first of all, our goal is to read from a file, for that we need to get the file's file descriptor, i'll be putting some ressources down bellow.
After getting our file descriptor we should check if it's a valid one or not, if it isn't valid we return null and our program will quit.
If our file descriptor is a valid one we will send it to our "miread" function, this function's role is to give us a string that contains a '\n' new line  (this means a line XD).</p>
The string we'll be getting from miread will be :
<ol>
	<li>a string with no new line, in this case the file we are reading from only contains a long text with no new lines.</li>
	<li>a string with new line, in this case there are 3 possible values :</li>
	<ol>
			<li> the string contains the new line in the middle of it's char's, this means that we will be having a "reminder" (we will be talking about this afterwards).</li>
			<li> the string contains a new line (\n) at the end.</li>
			<li>the string contains only a new line.</li>
	</ol>
	<li> (NULL), in this case we either had a problem during the execution or we reached the end of the file.</li>
</ol>

When we have a reminder in our hands we keep it in our static variable (you need to search a bit heheee) so we can use it when call get_next_line again.
This remainder will be either :
<ol>
	<li>NULL, if we reach the end of file, or we faced a probleme during the execution.</li>
	<li>a string that may or may not have a new line :</li>
	<ol>	
		<li>if our remainder contains a new line this means that we won't be reading, but rather filltering our remainder to extract the line we still have in our reminder.</li>
		<li>if our reminder dosen't contain a new line, we will be reading and joining what we read with our remainder to check for the possibility of having a new line.</li>
	</ol>
</ol>
<p>
I've been talking about the filltering process for a while,you should be curious how we will be filltering (heheeee this is where the fun beggins).
To fillter we will always be having two values wich are as you probsbly guessed "line" and "reminder", to get this values i struggeled a lot to keep them and also the check if they were valid or not, so with on of my peers (yel-mrab) we used a structure called s_data (you will find it in the .h file (header)), so this structure we'll be holding for us the values of the line ,the reminder and the fail_checker.
This struct contains 3 variables, two char pointers that will be having the address's of the allocated memory generated by our fillter and miread functions and also an int that we be taking two values (1 or 0) if the value is 1 this means the program should exit, if the value is 0 we will continue the process.
<br>
Last but not least, we will be talking about leaks, to avoid leaks we will be using two functions the mifree_data and the free.
Mifree_data function was made to free the struct's content (the heap addresses holded by the two pointers) when a problem occurs and set the fail to 1.
Mifree function was made to free two strings at once, to use less lines (XD).
<p>
And this is what are code is all about, hope you learned something ^^.

<h2>ressources :</h2>
                        <a href="https://stackoverflow.com/questions/5256599/what-are-file-descriptors-explained-in-simple-terms">file descriptor</a>
			<hr>
                        <a href="https://www.geeksforgeeks.org/input-output-system-calls-c-create-open-close-read-write/">read systeme call</a>
                        <hr>
			<a href="https://www.tutorialspoint.com/where-are-static-variables-stored-in-c-cplusplus#:~:text=Static%20variables%20are%20variables%20that,is%20the%20entire%20program%20run.&">static variable</a>
															<h3>Iel-bakk</h3>
