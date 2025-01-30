<h1>SOAR EDR Project | Intro</h1>

<p align="center">
<img src="https://snipboard.io/xnGfQg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to the final part of the series on the SOAR EDR project. If you haven't seen the previous parts where we go over how to build out a workflow for this project, set up LimaCharlie, create our detection rules, and establish a link between Tines and LimaCharlie, I would highly recommend you go and see that first.
<br />
<br />
<img src="https://snipboard.io/sF73ZI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
Today's objective is to create our playbook by following the workflow that we created back in part one. As a reminder, the objective for this Playbook is to send a Slack message and an email containing important information about the detection that LimaCharlie had generated. Tines will prompt the user if they want to isolate the machine. If the user selects yes, LimaCharlie should then automatically isolate the machine. Let's get started.
<br />
<br />
<img src="https://snipboard.io/sF73ZI.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<br />
<br />
<br />
<br />
  let's begin by reviewing our workflow that we created in part one so currently we do have the infected host with the detect hack tool that lima charlie had detected it then pushed it over to times which we can confirm because if we click on the retrieve detection select events we do indeed see our event here just like so so if we take a look at the workflow tines is going to be sending a message over to slack so this means there must be a link between times and slack to do that we'll head over to slack and let's take a look at some of the applications here we can click on more and automation and let's search for times perfect I'll click on ADD and let's take a look here it says automate workflows across your business with tin and slack allowing you to focus on what matters we have workflows that you can build receive and approve or deny requests via slack create new channels and send messages so send messages is what we want so I'll go ahead and add this to slack and scrolling down steps for installing the application log into your 's tenant navigate to the team that will be using the API and click credential click plus new credential and select slack okay let's head over to our times and I'll open up a new tab and go into my dashboards so the instruction said navigate to the team that is going to be using the API and click credentials so this is our team and we see credentials here and new credential scroll down is there a slack here and there is perfect so so we have the option to select our own slack app or we can use Ty application for slack I'll use Ty's application tin will be able to view the content and info about channels and conversations these are just the permissions that this particular bot has access to and I'll click allow and now we have a new slack credential that we can use so this essentially establishes the link between times and slack if I head back over to the story and let's select templates and grab our slack so I'll drag that in here and on the right I'll search for message we want to send a message so send a message here yeah let's go ahead and select that and the action is going to be send a message description post a message to a public Channel private Channel or DM it must have the permissions of chat right so what we want to do is send a message over to our slack but I did create a channel called alert and if we take a look at slack go into our home make sure that you're selected alerts right click it and click on view Channel details we want all of the detections being sent over to this alert channel if we look at the bottom there's the channel identifier or Channel ID go and copy that for the channel SL user ID field I'll go ahead and paste that in like so and the message will be hello you are awesome and it says connect to Slack so I'll go and select that but we do get this use our own slack app and 's app we're already connected to Tin so let me close this out and then I'll just click out of the screen we do see credentials here that we just added so go ahead and select connect and there is no connection what if I type in slack okay so what we need to do then let's go ahead and refresh this now let's try and connect it still none okay let's go back over to our dashboard go into our credentials we do have it here okay then that means let me just create a new story then for some reason it's not capturing my credentials create a new story I'll rename this to my defer ds- EDR project and now let's try and put in the web hook I'll say retrieve detections description is retrieve the ma charie detections so that means I'll have to recreate the web hook again so I'll copy the web hook URL and head over to my lima charlie go back over to outputs and I'll delete this one here or actually I think I can just edit it yeah I'll just paste this in save output view samples likely nothing because we need to regenerate it on my server I'll just hit the up Arrow key and hit enter so that should generate the detection refresh the sample and I do see my detection here so going back over to times I'll click on events and we do see two events expand the first one bodies and we got our detection perfect so now we know that this web hook Works go over to templates slack drag it in search for message click on send a message and back to the channel ID of slack I'll copy the alerts Channel ID paste that in here the message is going to be your awesome yeah now it's connected okay beautiful so you know what let's connect this and let's just test it so I'll click on run and if we click on statuses we do see it as working clicking on logs sending requests to the channel head over to the channel and look at that perfect so this is good to go now we know TI and slack can talk to one another going back over to the workflow here so we know that this particular section is good we just need to start sending the message with details and the message with details contains the time computer name Source IP and all this other good stuff now before I do that let's actually build out the email portion now so it's essentially going to send the same thing but this time towards the email back over to the times I'll click on slack and it's currently spinning so let's try and stop that here why are you spinning so it just took some time looking on the left we do see an action for send email so I'll drag this into the storyboard and I am going to connect the retrieve detections over to the send email for the send email we'll click on the build tab if you're not already in there description we can type whatever we want but I'll just say send email for the email I'm actually going to use a temporary disposable email address from squarex and I'll go ahead and copy this one head over to my ties and paste in the temporary email I absolutely love the domain typing squirrel I'll change the reply to the sender name is going to be alerts and let's just say the subject as test for now and the body is going to be a real email body could go here just for now and I think that's pretty good let me click on options we don't really need an attachment so we have the variables here sending email okay I'll go ahead and test this and I can test this using the retrieve detection web hook so I'll click on the latest one you can tell by the ID as it increments by one notice how it ends in 797 and this one ends in 796 so the 797 is the most recent one I'll click on test and then let's take a look at the email so we get one here and we also get one here not sure why it's sent twice but it's sent nonetheless so that's pretty good so now we have confirmation from the web hook we can send it over over to slack and we can send an email to ourselves looking at our workflow the next thing to do is the user prompt so does the user want to isolate the machine yes or no and to use a prompt we can actually use what is called a page which is under the tools you can click on page and drag that into the storyboard so I'll name this page as user prompt user prompt for the description I'll say isolate computer yes slash no and who has access to this members of this time tenant yeah that sounds good anonymize I don't think so I think that's okay page behavior Show success message redirect to URL or move to the next page I think we'll just select show success message and regardless of the user action that they select a success message will show up and currently the success message is thank you for your submission I'm going to just replace this to close this window should I say thank you too thank you close this window H thank you you can now close this window beautiful and yeah so let's link this together and we can click on edit page in here we can go ahead and edit our page so I'll type in my deer Das s- ADR project and if we take a look at our workflow it's essentially asking if the user wants to isolate the machine or not so let's put in a message here that makes sense I mean we can just say do you want to isolate the machine and on the left hand side we have a button right here or there should be another booing there you go so this will essentially tell me yes or no so let's put that up here and I'll change Boolean to isolate now that I think about it the contents above should contain our interesting fields and our interesting Fields is going to be these ones right here so the time computer name source and all that good stuff that way when the user is presented with this page they don't have to move back and forth they can just see everything on one page I think that's a great idea so that's what I'm going to do for now let's go ahead and just click on back and before we move on let's now includ the details into our messages so for the interesting Fields I'll go ahead and select web hook go into events and let's expand our detection I'll go into body and these are all of the fields that we can use so in my message with details that is going to be sent over to slack and the email I did mention time computer name Source IP process command line file path sensor ID and the link to the detection but I noticed that I didn't include the title of the detection itself so I think that is a good idea to include so let's go ahead and include this and this is using the field called cat so what I'll do is I'll open up a notepad and just add these in here so cat is the title we do see a link so this is going to be our link to the detection so I'll type in link this is detection what else here I think that's pretty good here so let's click on the detect click on event and we're interested in the command line so I'll copy this now I wonder if this just copies the path oh nice it copies the actual path and not the value so this does all the easy work for me okay let me just copy the cat and then I'll copy the link what else do we need I did say computer name where is the computer name let's expand routing and we do see our host name so this is the computer name I'll put that there what else time the event time now this is an Epoch format and if you don't know what that is it's essentially the number of seconds that have elapsed since January 1st 1970 and to convert it you can go over to EPO converter.com and if we paste that in click on timestamp to human date we get our human date timestamp in GMT UTC time so I will copy the event time and paste that in here I also said I wanted the source IP address now we have two of them we have external IP and we have internal IP in your instance if you're not running it in the cloud and you have a private IP this is the one that you want you would rarely want the external IP to be honest so just select the internal IP I'll copy that paste that in and process we want our process looking at the available field names it looks like it doesn't have the field name of process the next best thing is the file path itself self so I'll use the file path and then all we're missing is the sensor ID if we go back to routing there should be an ID here yeah S ID sensor ID I'll copy that and then I'll paste it in here so these are the fields that we're interested in the cat is the title this is the detection and then we have our command line host name event time internal IP address the file path and the sensor ID now if you received this message you would probably want to see it in a proper order you probably don't want to see the information all over the place so I'll go ahead and start reorganizing some of these we'll start off with the title because that is the most important the second most important in my opinion is the time of when this occurred we want the host name so where that it occurred and now that I say this we should also include the username so let's copy that I did not have the username in my original workflow so this is what I meant by it is perfectly normal to make a lot of changes to your workflow so I'll put in the username and then we also want the IP address so I'll put the IP address underneath the host name that way it's a lot more consistent we'll put the file path which is the process and then I'll put in the command line finally we'll put in the sensor ID and we'll end off with the detection link that way the user can just click on it if needed this is a lot better so now what we can do is likely just copy this and if we head over to our slack message notice how our message here is you're awesome we could in theory just paste in our variables just like that now let's say that we didn't do that what we could have done is click on the plus and we have two options we have value and tag if you click on value it provides you with either a data or a key and if we scroll down it'll also provide us with a Boolean so true or false now what we're interested in is the data and if we take a look at the value that is under the data you'll notice that it is our web hook the retrievor detections now the reason why we see this is because there is a link between the web hook over to our slack if I were to remove this link you will not see this retrieve unor detections as a matter of fact let's try this out I'll remove this and now clicking on slack click on the plus button value I no longer see it so if you're running into any errors do make sure that your lines are connected just like so my web Hook is now connected to slack I'll go ahead and remove this message click on the plus value and now I see my web hook so let's click on that I want you to take a look at the bottom right here you'll see your results if you see results you know you're on the right track so let's go ahead and click into this and then from here we have three data points the body headers and response now if we click on our web hook click on event and we'll expand the retrieve detections these are the three things that we saw earlier the body headers and response all of our data is within the body so what this means is that if we go into slack and go into our messages click on value go into retrieve detections we'll want to select body click on body again again and here is all of our information so if we go ahead and select cat we should see our detection title and we can see that under the results at the bottom right the one thing I don't like about times is that we can continue to drill into that particular field for example we do see our detection title here but if I were to click into cat again it's now an incomplete path and that is because we have a DOT right so it's essentially just continuing to drill into it and if you do happen to get an error just double check your path always pay attention to the bottom right as that is a telltell sign whether or not you're on the right track I'll go ahead and remove the dot and now I see my title so I know that this is good to save it like this just click out of the screen and now you're good however I will go ahead and remove this and I'll just paste in all of the values that we copied and now let's just try it so I'll click on test and we'll test with the retrieve detection web hook click on test head over to alerts in slack and look at that that is pretty cool we have our title we have our time we have the computer name the source IP now because we are the ones that are building this we know exactly what these fields are but if you pull Bobby from the street Bobby would not know what the heck he's looking at so let's make this a little more userfriendly and to do that I'll just go ahead and remove all of this and back into my notepad I'll say title time computer let's say Source IP username the file path the command line sensor ID and finally the detection now let's say detection link there you go so I'll copy this and I'll paste it in the message now let's try and test this out nice this looks a lot better so we see our title time computer Source IP username file path command line sensor and detection link so if I were to click on this we get redirected straight into the process wow and then we can look at the surrounding events plusus 5 minutes what led up to this process was there any network connections afterwards any persistence created that wow okay okay I don't know about you I get extremely excited whenever I see something like this so our Slack works perfectly fine now we can bold these field names but that's a little extra for now let's just keep it as this the next thing to do is start to send an email that contains this information lucky for us we already have the message copied let's go ahead and paste that in and let's try it out run test retrieve test over to my emails click on test and ooh that doesn't look as good so it doesn't render our new lines it must use I guess HTML so that means we'll need to change this a bit is it HTML open an HTML editor yeah it is okay so what we can do is just I believe it's BR yeah okay so we'll just add in BR now if you're not sure you can always just Google HTML new line to do a line break in HTML use the BR tag and that is simply what I'm I'm doing BR and that I'll just copy this oh so much easier but I messed up and did not copy the closed Arrow there you go so if I were to get an alert this is what I would have I don't like the detection link H what if I do another BR that looks better I'll keep it like this so let's click out of the screen and let's do a test click on our new email sweet okay so this is working as expected and unfortunately this is not a hyperlink but that's okay I can just highlight it and go to it nice okay so the next thing to do is our user prompt does the user want to isolate the machine yes or no what we can do is because we already have our message does this page use HTML that is the question I don't think it does but let's just check so here there's nothing that says HTML what that means is I I can just copy this and I'll paste that in here this is looking pretty good I'll click on back and is there a way to test maybe visit page click on this nice so we do have our title time computer source user all our good Fields here the detection is hyperlink and that brings us straight to Lima Charli now isolate yes or no so if I say no and hit submit thank you you can close thiswindow I like it I like it looking at our workflow we are now at this section here so let's build out the no first now the user selects no the computer computer was not isolated please investigate and this gets sent over to slack so that means we need to use the computer variable which in our case is this right here routing host name okay let's build that we first need a if else H trigger I'll bring the trigger here and we do see our Trigger action so let's just say no and I'll connect this over and for the rules I'll click on the plus click on value and we want the user prompt at the bottom right we see results so that's good click into it and click into the body body and I believe it's under isolate so right here it says false now this is taking the data that I just ran so if you recall I selected no and hit submit if you don't see this option go back to your page click on visit page and actually just follow through the scenario that way it will generate the events for you but yeah going back over to my data here I'll click on isolate or or I already clicked on isolate so that's good and if we take a look at the bottom right we do see result equals false now if you do make a mistake and actually click into it look at the bottom right you'll see error so go ahead and remove the dot and you get your actual Boolean which in this case is false so I'll click out of the screen and right here it says isolate is equal to I want this to be equal to false right so if isolate equals to false I just notied I have a period here I don't want this period because if we have this period again look at the bottom right we get an error and we don't want that so remove that and we don't have a period so we're good isolate is equal to false then do this the do this action is going to be another slack message and we can copy this and contrl V to paste it now we already have our Channel ID and the message but we don't need this exact message here instead if we look at our workflow it says the computer computer was not isolated please investigate so that means I'll go ahead and remove all of this except for the computer and remove the title and time and then I'll type in the computer and then I'll remove this computer keep the variable was not isolated please investigate yeah that looks good now let's connect this over to our slack and starting from the page or actually no starting from from the trigger we'll go ahead and test it so I'll click on test it's going based on the user prompt that we just completed so I'll click on that and test so now in theory our slack should receive a message and it did not okay so if I click on the trigger we need to understand why it did not I'll click on the events there is no events perhaps I need to begin from the user prompt instead so I'll click on user prompt click on visit page and then I'll just rerun the recent event that's so strange let me visit page again why is it automatically completing it you know what I'll click on visit page and just select the previous one which is 796 instead of 797 so here we go I'll click on no actually before I even submit it you because you might not have the same information as I do so if you don't have a second event you can just generate It Again by clicking on the web hook click on event and all we need to do is just select the first event and click on re-emit when you click on Reit it essentially reruns the same detection for you so now our slack should have a new message we should also have a new email right here and if we take a look at our user prompt when we click on visit page we should have three or well in your case you might just have two of events but in my case I have three events and I can select the just now and now I am at this page so it ask me do I want to isolate no I do not go ahead and submit in theory we should now have a new slack message sweet it does so the computer mydefer sore EDR was not isolated please investigate that is fantastic okay so we got this good to go now it is time for this section the fun part the automated response so when the user selects yes lima charlie will then go out to isolate the machine this means we'll need to add in another trigger so I'll go ahead and copy this and I'll paste it in but we need to know exactly what's the field value when the user select yes and what we can do is just rerun it so let's click on web hook I'll click on events select the first one and Reit head over to the user prompt click on just now and and I'll say yes submit close that out and if I click on the events and user prompt body so once you hit submit the isolate is actually true so it's not false anymore so what that means is on let me go back and drag that there connect it to my no connect this I'll change the no to yes and I'll change the false to true so now if the user selects no it's going to go down this path of sending a message to slack if the user selects yes it is going to then move on to lima charlie what we can do is take a look at the templates and see if there's a lima charlie and there is so I'll click on that and drag it okay it doesn't seem like it wants to drag let me do that one more time Li Charlie drag there you go and taking a look at the workflow it needs to isolate the machine so how does lima charlie know which machine to isolate and if I were to think about it it is likely due to the sensor identifier this one right here this field name of sensor ID because that's generally how edrs tend to work so I'll select the template and let's see if we have a contain or isolate isolate sensor o isolate censor in Li Mae and isolate centor what's the difference One is using a sensor unor Sid and then the other one is using a sid okay I'll just select the first one for now and if it doesn't work we can select the second one so this one says isolate sensor that is good and it's using the variable of Sid now if we take a look at our notepad our variable is retrievor detection. body. detect. routing. Sid it's not just Sid so let's go ahead and copy this and let's paste that in here so I'll remove Sid paste it like that I think that's good now let's connect that over to the isolate sensor and if you want it to be 100% sure you can actually just remove this and do this manually so I'll click on the plus button click on value and because it's connected to everything else we can actually select retrieve detections which is the very first web hook so I can click on that let's drill into it and under the body body under routing and I believe it's here yeah Sid now here's a common scenario that I often see if you introduce a new action or an application sometimes you'll need to reun the Playbook to actually generate the events what do I mean by that well if you take a look at the bottom right we currently see the results as null that is because we need to rerun The Playbook while being connected to the new application or action which in our case we just introduced the new action of isolate sensor so I'll scroll back up here and let's go ahead and click on events the first one remit and under the user prompt I'll go ahead and select visit page just now yes I will select yes to isolate and this time it will feed it down this way and we can already see that there's an error which makes sense because we didn't really do any configurations to it other than changing the URL but what I wanted to demo for you is that if I now click on the plus button click on value retrieve detections body body body routing routing and Sid at the bottom right we now have our sensor ID isn't that awesome so I'll leave that as is and O we do see a DOT right if you take a look at the variable we see a period and we don't want that so let's click on it and remove it and now we're good or no we're not good this is not selecting our sit why is it not selecting the sit so just like this right routing. sit the method is post content is Json and it's looking for a credential dolore Charlie so what this tells me is that we need to authenticate and if we take a look at the credentials we do need to connect it using a credential so what I'll do is I'll open up a new tab and let's click on credentials I'll click on new credential at the top right and let's see if there's a lima charlie credential here so we see crowd strike um doesn't seem to be a lima charlie okay that's okay what we can do is go ahead and Google Lima Charlie's API so I'll type in Lima Charlie API and in Lima Charlie's documentation I'll click on the first link it says simply issue an HTTP post request such as C- capital x post towards this domain it says here this makes user API Keys very powerful but also riskier to manage therefore we recommend using organization API Keys whenever possible so what we can do is use an organization key and you might ask how do I get the API key from Lima Charlie well if you head over to Lima Charlie go into access management and there is the rest API and here we have the API details and the user generated API Keys we also have service manage apis and and ingestion so in Lima Charlie's documentation they mentioned to use the organization apis key whenever possible and here it appears that there's already a JWT we can just go ahead and select that and you know what let's try it out so if I were to select the credentials click on new credentials is there a way to just add this as a variable so let's try text and for the name I'll just type in Lima Charlie name Charlie API and I'll paste in the value here for the domains I am going to select as. Lia charli. what this is doing is essentially ensuring that the credential can only be used towards this site and its subdomains and then let's go ahead and hit save nice so we have our credentials here and back over into our story I'll go ahead and refresh this and now what we can do is click on connect select our credentials that we just created and now our isolate sensor should have valid credentials I say we go ahead and try it out currently if I go into my lima charlie and go into my sensors if I were to isolate it we would not see this as allowed so let's go ahead and rerun this so I'll just click on the first event on the user prompt click on Reit and then that means it should go into success go into yes click on events and isolate sensor status 200 this is looking pretty good body body is nothing header okay why am I looking at this we can look at lima charlie refresh this that is awesome so we got this as isolated that's so cool so if I go into my vulture and click on view console I shouldn't be able to ping or anything so let me go ahead and hit the control alt delete so this is it right here moment of truth I will ping my dfir docomo this to the network so now we have network access and I'll go back into my server type in CLS to clear out the screen let me try pinging my defer again so we do have a reply here and you know what let's do an endless ping how about that so that that way we know it's pinging and if I were to rerun this entire web hook so let's do this from the start I'll click on events the first one remit so we do get our alerts here right here and then we should get a new email like so and finally our user prompt I'll go into the visit page just now and then it says my dfir s- EDR project gives me all of the important information here as well as the detection link and then isolate yes or no let's do it click on Yes actually before I click on that take a look it is pinging it's good so click on yes and submit thank you you can close this window so let's close it out and our isolation should work now if we go back over we should start seeing a general request or general failure actually awesome going back into lima charlie refresh it we do have it as isolated so let's rejoin this and now we are near the end looking at our workflow once the user selects yes lima charlie will then go and isolate the machine this section right here is completed the last section is that a message containing the isolation status and the computer computer has been isolated and that gets sent over to slack once it's done isolating it sends a message so let's copy slack and I'll paste it in I'll now connect lima charlie over to slack and we already have the computer variable so I don't need to select that but what did our message say the computer has been isolated okay the computer has been isolated and then it also included the isolation status so I'll say isolation status and I'll click on the plus button click on value and we want to click on isolate Center and on the bottom right let's go ahead and hover over that and it doesn't really tell us if this was successful or not I mean we can based on the 200 we can assume that isolation was completed but instead let's see if there is a lima charlie application so go into templates lima charlie and when I said application I meant template so is there a get isolation oh here we go get isolation status awesome this is exactly what we're looking for so I'll go ahead and connect this over to get isolation status change my Sid I'll click on the plus button value retrieve detections go into body go into routing and then go into Sid we don't want the period so that is good the method is get it's using the credential of lima uncore charlie so we need to be careful of this this is not the credential to use and to change that we simply just need to click on connect under the credentials and select lima charlie this will automatically update this so just make sure of that just in case you accidentally Overlook that and once you get isolation status we should be good so let me go ahead and test this out and we'll use the isolate sensor which is this event here the most recent event from that so I'll test that out and the get isolation status we do see a status of 200 let's go into body is isolated false that is the field that we want so isolation status I'll click on the plus button value get isolation status now if you take a look at the bottom right we see it as null remember what I said earlier we need to rerun the playbook in that case so before we do anything let's rerun it and I'll actually start from the user prompt because we don't need to send a message to slack or an email so I'll click on events the first one click on Reit actually before I do that is it allowed yes it is okay and then I'll click on Reit so the sensor should be isolated let's take a look so that's isolated and then get isolation status and then it will send it over to slack so we take a look at slack wow okay we get a big isolation status so we need to change that but we do have the message of the computer my defer sore EDR has been isolated we're close I'll remove the get isolation status click on the plus button value I'll click on this get isolation status click on that again and it was under body I believe yeah so if you take a look at the results it says is isolated true so I'll click on body and I'll use the field value of is isolated because we want to know the status of the isolation and currently the bottom right it is set to true so Moment of Truth here I will go ahead and rejoin the network and what I'm going to do is just to make sure everything's nice and clean is just put a bunch of A's that way we can see the new message so from the beginning we click on events click on the first event Reit our workflow should now kick off we should get a slack message which contains the information related to the alert we should also get an email which yeah we get it right here and then from the user prompt let's go ahead and visit the page and we get it here isolate yes submit and I'm just going to go straight to slack look at that isolation status true the computer might dfir sore EDR has been isolated if we go into lima charlie refresh isolate it if we go into our server I haven't stopped this ping whoops let's say pingy youtube.com this time General failure that is amazing aming if you followed along congratulations on completing the sore EDR project this project was not easy and you should be extremely proud of yourself for tackling this we went over a ton of material and it can get pretty technical very quickly if you enjoyed this project I would highly recommend that you take a look at my other projects that I have such as the sock automation project and the active directory project if you haven't done those already if you're an aspiring blue teamer specifically within the security operations domain you'll love the sof course that I have created and again I'll leave the link down for you to check it out if you're interested it would mean a lot to me if you shared this project with other aspiring analysts who want to get some hands-on experience with that being said thank you so much for watching and I truly hope that this has helped some of you remember to stay curious and do things differently
