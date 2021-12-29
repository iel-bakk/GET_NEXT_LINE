# **************************************************************************** #
#                                                                              #
#                                                         :::      ::::::::    #
#    pinters.txt                                        :+:      :+:    :+:    #
#                                                     +:+ +:+         +:+      #
#    By: iel-bakk <marvin@42.fr>                    +#+  +:+       +#+         #
#                                                 +#+#+#+#+#+   +#+            #
#    Created: 2021/12/21 14:13:18 by iel-bakk          #+#    #+#              #
#    Updated: 2021/12/21 15:03:04 by iel-bakk         ###   ########.fr        #
#                                                                              #
# **************************************************************************** #

In this readme file i'll be talking about the code of my "get next line" project so read carefully ^^.

But, before we dive into the code let's discuss the big picture of this project.

This project is part of the 42 cursus commun core. (GET_NEXT_LINE)
NOTICE : if you're also working on this project please check if the subject is the same as this one to avoid any problemes.
        even if your not a 42 student you can do these project to undrestand new concepts.
        and happy coding (iel-bakk).
I'll start talking about what get next line would add to your knowledge see, the get next line project aims to teach you :
first : how to read from a file using a file descriptor.
second : what are static variables.
third : how to avoid leaks.

These are what i think the things that you will need to know in order to get this project done.
The emplementation of these tools (the three thing i mentioned above) in your code will be the judge of your codes quality.

As i said before the goal of this project is to be able to read a file line by line with the '\n' included in each line.
So as a first step we need to get our hands on The file descriptor wich will be our key to access the file in order to read from it, this file discreptor is something that we get by calling the system call (OPEN).
After getting our file descriptor with the reading permission we can read form our file, but while reading ther are some rules that we must not break and these rules are :
        YOU CAN READ ONLY A LIMITED NUMBER OF BYTES EACH TIME YOU CALL READ.
        THE NUMBER OF BYTES YOUR ALLOWED TO READ IS DEFIEND BY THE USER (BUFFER_SIZE).
        EACH TIME YOU CALL YOUR GET NEXT LINE FUNCTION YOU MUST RETURN A SIGNLE LINE WITH THE NEW LINE INCLUDED.
        CALLING YOUR GET NEXT LINE FUNCTION IN A LOOP SHOULD PRINT THE WHOLE FILE CONTENT.
        UR CODE MUST NOT HAVE ANY LEAKS.
these are the rules concerning the output of our function (mandatory part ofcours).

Now, we can dive thru our code.

So first of all, our goal is to read from a file, for that we need to get the file's file descriptor, i'll be putting some ressources down bellow.
After getting our file descriptor we should check if it's a valid one or not, if it isn't valid we return null and our program will quit.
If our file descriptor is a valid one we will send it to our "miread" function, this function's role is to give us a string that contains a '\n' new line  (this means a line XD).
The string we'll be getting from miread will be :

1 : a string with no new line, in this case the file we are reading from only contains a long text with no new lines.
2 : a string with new line, in this case there are 3 possible values :
			#2-1 : the string contains the new line in the middle of it's char's, this means that we will be having a "reminder" (we will be talking about this afterwards).
			#2-2 : the string contains a new line (\n) at the end.
			#2-3 : the string contains only a new line.
3 : (NULL), in this case we either had a problem during the execution or we reached the end of the file.

When we have a reminder in our hands we keep it in our static variable (you need to search a bit heheee) so we can use it when call get_next_line again.
This remainder will be either :
1 : NULL, if we reach the end of file, or we faced a probleme during the execution.
2 : a string that may or may not have a new line :
			#2-1 : if our remainder contains a new line this means that we won't be reading, but rather filltering our remainder to extract the line we still have in our reminder.
			#2-2 : if our reminder dosen't contain a new line, we will be reading and joining what we read with our remainder to check for the possibility of having a new line.

I've been talking about the filltering process for a while,you should be curious how we will be filltering (heheeee this is where the fun beggins).
To fillter we will always be having two values wich are as you probsbly guessed "line" and "reminder", to get this values i struggeled a lot to keep them and also the check if they were valid or not, so with on of my peers (yel-mrab) we used a structure called s_data (you will find it in the .h file (header)), so this structure we'll be holding for us the values of the line ,the reminder and the fail_checker.
This struct contains 3 variables, two char pointers that will be having the address's of the allocated memory generated by our fillter and miread functions and also an int that we be taking two values (1 or 0) if the value is 1 this means the program should exit, if the value is 0 we will continue the process.

Last but not least, we will be talking about leaks, to avoid leaks we will be using two functions the mifree_data and the free.
Mifree_data function was made to free the struct's content (the heap addresses holded by the two pointers) when a problem occurs and set the fail to 1.
Mifree function was made to free two strings at once, to use less lines (XD).

And this is what are code is all about, hope you learned something ^^.

ressources :
                        file descriptor : https://stackoverflow.com/questions/5256599/what-are-file-descriptors-explained-in-simple-terms
                        read systeme call : https://www.geeksforgeeks.org/input-output-system-calls-c-create-open-close-read-write/
                        static variable : https://www.tutorialspoint.com/where-are-static-variables-stored-in-c-cplusplus#:~:text=Static%20variables%20are%20variables%20that,is%20the%20entire%20program%20run.&                       text=All%20the%20static%20variables%20that,known%20as%20the%20BSS%20segment).


											iel-bakk (1337 student); 

