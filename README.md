# StoryByNAO: Autonomous System Project Assignment
maintained by @nntdoan
This module read a story out loud with a virtual NAO robot perform gesture along side. 
Written in Python 2.7.18, tested in Window 10 x64, Choregraphe v2.1.4, for NAO virtual robot.

## Required packages
Please install the required packages by cd to the directory of the project and use the command:

> pip install -r requirements.txt

Also note that make sure you using the pip of your Python 2. </br>

We assume you have NAOqi stk working already, and that you are more or less farmilar with working with Choregragh from Python. Please make sure, when running this module, you call for your Python 2 instead of Python 3. </br>

Older version of Python 2.7.x (so x < 18) may interfere when you try to install the required packages. It is highly recommended you use Python 2.7.18. </br>

## Guide to run the module
From cmd, first you will need to cd into the directory of the project. </br>

Then you can run the story2NAOv2.py, which is the entry point to read a story with NAO perform gestures alongside. The command to read our group (18) story should be rather similar to this:

> py -2 story2NAOv2.py --port 4747 --storyfile "storyg18.txt"

Arguments: </br>
--ip : Robot ip address, default="127.0.0.1" </br>
--port : Robot port number, default="9559" </br>
--path2story : Path to the directory where all story files are stored from root, default="./stories/" </br>
--storyfile : Name of the story file that is in the path2story that you want to read, default="example.txt" </br>

You will need to change the --ip and --port to yours, which can be obtained from Choregraph -> Connection -> Connect to ... </br>

The default story file "example.txt" is a version of Listing 1: Example annotated story, given in the Project Assignment.pdf by our instructor, Dr. Paul Vogt. </br>

## Instruction on how to contruct a story file

This module read the story marked with gestures. It commands a virtual NAO robot to perform these gestures in synchrony with an audio generated from the story text. </br>

It does not take care of the actuals stroke of the gesture, their order of appearance, etc. These are instead pre-define in the story file in a markup language as below. </br>

So altogether you should write your story with a markup language rather similar to: </br>

`<g-beat {body part} {do}>` text `</g-beat>` </br>

e.g `<g-beat hand hello>` Hi `</g-beat>`

This module supports the gestures and their meaning as below. It should gracefully deal with unknown gesture by using the default `<g-beat head emphasis>` instead. You can add new gestures following the instruction in the next section </br>

| do | body part | use to express |  
|---|---|---|
| hello | hand | greeting, hi, ciao, etc. |
| afraid | hand | oh no kind-of expression, etc. |
| anyone | hand | all, everyone, come on, etc. |
| big | hand | very, extremely, especially, giant, gigantic, etc. |
| emphasis | hand | important, on time, etc. |
| grab | hand | reach out grabbing a thing and look at it, etc. |
| offer | hand | let's shake hand kind-of expression, etc. |
| please | hand | I need help kind-of expression, etc. |
| point-forward | hand | hand forward, open up, you, your, etc. |
| point-to-body | hand | me, my, myself, I, etc. |
| run | hand | mimick running, run, walk, hurry up, etc. |
| small | hand | things that mean the opposite to big, etc. |
| think | hand | think, thought, head, etc. |
| agree | head | two consecutive nods |
| emphasis | head | very subtle, I assure you kind-of expression, etc. |
| look-around | head | I'm confused, look around, making eye contact with every one kind-of expression, etc. |
| nod | head | a single nod |
| point-front | head | look down and look up |

*IMPORTANT NOTE*: A perfect text parser is a whole project on its own and beyond the scope of this project. The text parser in this module is a very simple one that works on the space separate each element in the text, so all elements in your story should be single-space separated. Something like this:

`<g-beat head nod> Hello, </g-beat> I am <g-deictic hand point-to-body> a NAO robot. </g-deictic> My <g-beat hand emphasis> artificial </g-beat> intelligence allows me to make an adequate estimation of <g-beat head emphasis> <g-deictic hand point-forward> your </g-deictic> <g-iconic hand think> thoughts.`

It is recommended that you put all the stories in the directory ./stories/ but it's fine if you don't want to. Just remember to specify --path2file --storyfile correctly so Python can find the file.

## Guide to create and add a new gesture
Should the existing gestures does not meet your need, you can add new gesture very easily as follows. </br>

1. Make a Timeline that perform the gesture in Choregraphe, please consult Choregraphe documentation for this </br>

2. Export motion in Python to clipboard. So right click on the Timeline -> Export motion to clipboard -> Python -> bezier/simplified (does not matter). We recommend bezier because it works better. </br>

3. If this motion is mainly performed by NAO head, you should create a new .py file in gChoregraph/ghead and paste the result of 2. in there. </br>
Do similarly if the motion is mainly performed by NAO hand, but the .py file is put in gChoregragh/ghand instead. </br>

Then, delete all the unnessary things: import, Alproxy, etc. Only need to keep the assignment of the 3 list names, times, keys and their values. Note that this module only support movement of head and hand for now, so you should only keep the joint names that belong to head and hand. So delete anything that does not belong to head and hand (i.e. ankle, hip, etc. ) if you happen to copy that too from Choregrahe. </br>

They should cause no problem being there, and it is also easy to extend this module to control the whole body. This cleaning is mainly for better sanity and debugging. </br>
 
4. Name the .py file exactly like how you would use in a .txt story file. 
For example, if you want to use the new gesture as such:

`<g-iconic hand love> I love you <\iconic>` 

the .py file need to be named as love.py and is placed within the directory gChoregraph/ghand. </br>

Now you should be able to use this gesture! </br>
Enjoy your story time with NAO! </br>


 
