e<h1>SOAR EDR Project | Intro</h1>

<p align="center">
<img src="https://snipboard.io/xnGfQg.jpg" height="80%" width="80%" alt="Disk Sanitization Steps"/>
<h2>Description</h2>
<br />
<p align="center">
Welcome to the final part of the series on the SOAR EDR project. If you haven't seen the previous parts where we go over how to build out a workflow for this project, set up LimaCharlie, create our detection rules, and establish a link between Tines and LimaCharlie. I would highly recommend you go and see that first.
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
Let's begin by reviewing our workflow that we created in part one. So currently we do have the infected host with the "Detect Hack Tool" that LimaCharlie had detected. It then pushed it over to Tines which we can confirm. If we click on "Retrieve Detections". Select "Events". We see our event here.
  
If we take a look at the workflow, Tines is going to be sending a message over to Slack. So this means there must be a link between Tines and Slack. To do that, we'll head over to Slack and let's take a look at some of the applications. Click on "More". Select "Automations" and let's search for Tines.

I'll click on "Add". Taking a look here, it says "Automate workflows across your business with Tines and Slack, allowing you to focus on what matters. We have workflows that you can build. You can receive, approve, or deny requests, via Slack. Create new channels and send messages. So sending a message is what we want. So I'll add this to slack.

Scrolling down, we can see the steps for installing the application. Log into your tenant. Navigate to the team that will be using the API and click "Credential". Finally, click "+ New Credential" and select "Slack". Let's head over to our Tines and I'll open up a new tab. Go into my dashboard. 

So the instruction said to navigate to the team that is going to be using the API and click "Credentials". So this is our team and we see "Credentials". Click on "+ New credential". Scroll down, until you see Slack. 

So we have the option to select our own Slack app, or we can use Tine's application for Slack. I'll use Tine's application. Tine's will be able to view the content and info about channels and conversations. These are just the permissions that this particular bot has access to.

I'll click "Allow" and now we have a new Slack credential that we can use. So essentially, this establishes the link between Tines and Slack. Head back over to the story and select "Templates". Grab and drag "Slack". On the right, I'll search for "message". We want to send a message, so select "Send a message".

The "Action" is going to be "Send a message". The "Description" will post a message to a public channel, private channel, or direct message. It must have the permissions of "chat:write". 

So what we want to do is send a message over to our Slack and I did create a channel called "Alerts". Select "alerts". Right-click it. Click on "View channel details". We want all of the detections being sent over to this alert channel. If we look at the bottom, there's the channel identifier or Channel ID. 

Copy that for the "Channel/User ID" field. The "Message" will be "Hello, You're awesome!" For "Credentials", click "Connect". However, there's no connection. Let me just create a new story since for some reason it's not capturing my credentials.

Create a new story. I'll rename this to "MyDFIR-SOAR-EDR project". Let's put in the Webhook. For "Name", I'll say "Retrieve Detections". For the "Description", I'll say "Retrieve LimaCharie Detections".

So that means I'll have to recreate the Webhook again. I'll copy the web hook URL and head over to my LimaCharlie. Go back over to outputs and I'll edit it. I'll paste the URL. Save the output. Click "View samples". Likely nothing since we need to regenerate it. On my server, I'll hit the up arow key and hit "Enter". So that should generate the detection.

Refresh the sample. Now I see my detection here. Go back over to Tines. I'll click on "Events". We'll see two events. Expand the first one. Now we have our detection. So now we know that this Webhook works. Go over to "templates". Drag "Slack". Search for "message". Click on "Send a message". I'll copy the channel ID of Slack. Paste that in here.

The "Message" is going to be "You're Awesome". Now it's connected. So let's connect the two now. Let's test it. So I'll click on "Run". If we click on "Status", we see it as working. Clicking on "Logs". It says "Sending requests to the channel". Head over to the channel, so now its good to go. So now Tines and Slack can talk to one another.

Going back over to the workflow. So we know that this particular section is good. We just need to start sending the "Message with details". The "Message with details" contains the time, computer name, Source IP, and much more. Before I do that, let's actually build out the email portion.

It's essentially going to send the same thing, but this time towards the email. Back over to Tines. Looking on the left, we see an action for "Send Email". So I'll drag this into the storyboard. I'll connect the "Retrieve Detections" over to the "Send Email". 

For the send email, we'll click on the "Build" tab. For "Description", I'll say "Send email". For the "Email", I'll use a temporary disposable email address from SquareX. Head over to Tines and paste in the temporary email. I'll change the "Reply to". The "Sender name" is going to be "Alerts".

The "Subject" as "Test" for now. The "Body" is going to be "A real email body could go here" just for now. I think that's pretty good. I'll click on "Option". We don't need an attachment. I can test the "Send Email" using the "Retrieve Detection" Webhook. So I'll click on the latest one. I'll click on "Test" and then let's take a look at the email.

So that's pretty good. Now we have confirmation from the Webhook. We can send it over over to Slack and we can send an email to ourselves. Looking at our workflow, the next thing to do is the "User Prompt". To use a prompt, we can use what is called a page.

Which is under "Tools". Click on "Page" and drag that into the storyboard. So I'll name this page as "User Prompt". For the description, I'll say "Isolate Computer (Yes/No)". For "Access Control", select "Members of the Tines tenant". Leave Anonymize unchecked. For "Page Behavior", select "Show success message".

Regardless, of the user action that is selected, a success message will show up. Currently, the "Success message" is "Thank you for your submission". I'll replace this to "Thank you, you can now close this window". Let's link this together now.

Click on "Edit page". I'll type in "MyDFIR-SOAR-EDR-Project". If we take a look at our workflow, it's essentially asking if the user wants to isolate the machine or not. So let's put in a message asking if the user wants to isolate the machine. Attach a Boolean from the Input fields on the hand side.

Now that I think about it. The contents above should contain our interesting fields. That way when the user is presented with this page, they don't have to move back and forth. They can see everything on one page. I think that's a great idea. So that's what I'm going to do.

Before we move on, let's now include the details into our messages. So for the interesting fields I'll select Webhook. Go into "Events" and let's expand our detection. These are all of the fields that we can use. So in my message with details that is going to be sent over to Slack and the email, I did mention time, computer name, Source IP, process, command line, file path, sensor ID, and the link to the detection.

However, I noticed that I didn't include the title of the detection itself. Let's go ahead and include this. What I'll do is open up a notepad and add these in here. So "cat" is the title. "Link" is "detection". Let's click on the "detect". Click on "event". We're interested in the "COMMAND_LINE", so I'll copy this.

Let me just copy "cat". Then, I'll copy "Link". Aswell, as "hostname". The "event_time" too. So this is an Epoch format. It's essentially the number of seconds that have elapsed since January 1st, 1970. To convert it, you can go over to "Epochconverter.com". If we paste the "event_time". Click on "Timestamp to Human date". 

We get our human date timestamp in GMT/UTC. So I'll copy the "event_time" and paste that in the notepad. I said I wanted the source IP address. However, we have two of them. We have external IP and we have internal IP. In your instance, if you're not running it in the cloud and you have a private IP, the internal IP is the one you want. 

You would rarely want the external IP to be honest. So just select the internal IP. I'll copy that. Paste that in the notepad. Looking at the available field names, it looks like it doesn't have the field name of the process. The next best thing is the "FILE_PATH" itself. So I'll use the "FILE_PATH". Now all we're missing is the sensor ID.

If we go back to routing, there should be a sensor ID. I'll copy that and then I'll paste it in the notepad. So these are the fields that we're interested in. The "cat" is the title. The "link" is the detection". If you received this message, you would probably want to see it in a proper order. You probably don't want to see the information all over the place.

So I'll go ahead and start reorganizing some of these. We'll start off with the title since that is the most important. The second most important, in my opinion, is the time. We want the hostname. So where did it occur? We should also include the username. So let's copy that. 

I didn't have the username in my original workflow. So this is what I meant by it, it is perfectly normal to make a lot of changes to your workflow. So I'll put in the username in the notepad. We want the IP address. So I'll put the IP address underneath the host name. That way it's a lot more consistent. We'll put the file path, which is the process. I'll put in the command line. We'll put in the sensor ID. We'll end off with the detection link.

That way the user can just click on it, if needed. This is a lot better. So I'll copy this and head over to our Slack message. Click the "+" on "Message". Now we have two options. We have "Value" and "Tag". If you click on "Value" it provides you with either a "Data" or a "Key". If we scroll down, it'll also provide us with a Boolean.

What we're interested in is the "Data". If we take a look under the "Data", you'll notice that it is our Webhook, "retrieve_detections". Now, the reason why we see this is because there is a link between the Webhook over to our Slack. If I were to remove this link, you would be able to see this "retrieve_detections".

As a matter of fact, let's try this out. I'll remove this. I'll click on "Slack". Click on the "+". Click on "Value". Now I no longer see it. So if you're running into any errors, do make sure that your lines are connected. I'll fix my WebHook connected to Slack. I'll go ahead and remove the message. Click on the "+". Select "Value". I'll click on "retrieve_detections".

I want you to take a look at the bottom. You'll see your results. If you see results, you know you're on the right track. From here, we have three "Data" points. The body, headers, and response. Now, if we click on our Webhook. Click on "Events". We'll expand the "retrieve_detections".

These are the three that we saw earlier. The body, headers, and response. All of our data is within the body. So what this means is that, if we go into Slack and go into our messages. Click on "Value". Go into "retrieve_detections". We'll want to select "body". Click on "body" again.

Now, here is all of our information. So if we go ahead and select "cat". We should see our detection title at the bottom right. The one thing I don't like about Tines is that we can continue to drill into that particular field. For example, we do see our detection title here. However, if I were to click into "cat" again. It's now an incomplete path.


That is because we have a dot at the end. So essentially, it's continuing to drill into it. If you do happen to get an error, just double-check your path. Always pay attention to the bottom right. As that is a sign of whether or not you're on the right track.

I'll go ahead and remove the dot. Now I see my title. So save it like this. Now let's try it. So I'll click on "Test" and we'll test with the latest "Retrieve Detections" Webhook. Click on "Test". Head over to "alerts" in Slack. Now we have our title, we have our time, we have the computer name, and the source IP.

Now, since we're the ones that are building this. We know exactly what these fields are. However, let's make this a little more userfriendly. To do that, I'll just go ahead and remove all of this. Back in my notepad, I'll say "Title:", "Time:", "Computer:", "Source IP:", "Username:", "File Path:", "Command Line:", "Sensor ID:", and "Detection Link:".

I'll copy this and I'll paste it in "Message". Now let's try and test this out. This looks a lot better. So we see our "Title", "Time", "Computer", "Source IP", "Username", "File Path", "Command Line", "Sensor ID", and "Detection Link". If I were to click on this, we get redirected straight into the process. Being able to see the surrounding events. What could see what led up to a certain process? Were there any network connections afterward? Any persistence created? 

So our Slack works perfectly. We can bold these field names, but that's a little extra. For now, let's just keep it as this. The next thing to do is send an email that contains this information. Lucky for us, we already have the message copied. Let's go ahead and paste that in and let's try it out.

Run "test". Click "Retrieve Detections". Go over to my emails. Click on "Test". Unfortunately, it didn't render our new lines. I'll try using HTML. So that means we'll need to change this a bit. Click "Open HTML editor". What we can do is add in "<br>". If you're not sure what the problem is next time, try googling it. 

So if I were to get an alert, this is what I would have. Let's click out of the screen. I'll click "test". Click on our new email. So now this is working as expected. Unfortunately, this is not a hyperlink, but that's okay. I can highlight it and go to it. 

So the next thing to do is our user prompt. Does the user want to isolate the machine? Yes or No. I can just copy this and I'll paste that in here. I'll go back. I'll click "Visit page". Click the lastest one. Nice. So we have our title, time, computer, source, user, and all our good fields here. I'll click the hyperlink and that brings us straight to LimaCharlie.

Now isolate? Yes or no. I'll test I out. So if I say "No" and hit "Submit". It says "Thank you, you can close this window." I like it. Looking at our workflow, we are now at "User Prompt" section. So let's build out the "No" first. So if the user selects "No", it will say "The computer was not isolated, please investigate." Again, this is sent over to Slack.

So that means we need to use the computer variable. Which in our case, is "routing.hostname". Let's build an "if else". I'll bring the trigger here and we see "Trigger Action". I'll replace it with "No". I'll connect this over. For "Rules, I'll click on the "+". Click on "Value".

Now we want "user_prompt". At the bottom right, we see results. So that's good. Click into it. Expand the body. I believe it's under "isolate". The result says that it is "false". Now this is taking the data that I just ran. So if you recall, I selected "No" and hit "Submit".

If you don't see this option, go back to your page. Click on "Visit page". And follow through with the scenario. That way it will generate the events for you. Going back over to my data, I'll click on "isolate". If you make a mistake, click on it. Look at the bottom right, you'll see "Error". So go ahead and remove the dot.

I'll click out of the screen. Here, I'll replace the "foo" to "false". Now we want the "Do this" action. The "Do this" action is going to be another Slack message. I'll copy and paste Slack, then edit it. 

Now we already have our Channel ID and the message, but we don't need this exact message here. Instead, if we look at our workflow, it says, "The computer was not isolated, please investigate." So I'll go ahead and remove all of this, except for the computer.

I'll type in "The computer:" and "was not isolated, please investigate." That looks good. Now, let's connect this over to our Slack. Starting from the trigger, we'll go ahead and test it. It's going based on the user prompt that we just completed, so I'll click on that and "Test". 

In theory, our Slack should receive a message. Unfortunately, it didn't. We need to understand why it didn't. I'll click on the "Events". However, there are no events. Perhaps, I need to begin from the "User Prompt instead. So I'll click on "User Prompt". Click on "Visit page" and then I'll just rerun the recent event.

So now, in theory, we should have a new Slack message. Sweet, we do. "The computer: mydfir-soar-edr was not isolated, please investigate." That is fantastic. So we got this good to go. Now it's time for the automated response section. So when the user selects "Yes", LimaCharlie will then go out to isolate the machine. This means we'll need to add in another trigger.

So I'll go ahead and copy this and I'll paste it. However, we need to know exactly what the field value is when the user select "Yes". What we'll do is rerun it. So let's click on Webhook. I'll click on "Events". Select the first one. Then, click "Re-emit".

Head over to the User Prompt. Click on the recent event. I'll select "Yes". Close that out. If I click on "Events". Expand "user_prompt" and "body". As we can see here, once you hit "Submit" the isolate becomes "true". So it's not false anymore. Let me connect the User Prompt to both of the Triggers. 

I'll change the "No" to "Yes". I'll change the "false" to "true". So now if the user selects "No", it's going to go down this path of sending a message to Slack. If the user selects "Yes", it is going to move on to LimaCharlie. What we can do is take a look at the templates and see if there's a LimaCharlie. And there is.


So I'll click on that and drag it. Taking a look at the workflow, it needs to isolate the machine. So how does LimaCharlie know which machine to isolate? The answer is the Sensor Identifier. I'll select the template and search up "isolate". Select "Isolate Sensor".

When we click on it, we can see that it's using the variable of "sid". Now if we take a look at our notepad. Let's go ahead and copy the one the says "sid". Let's paste that in the "URL". I think that's good. Now, let's connect that over to the "Isolate Sensor".

If you want it to be sure its correct, you can actually just remove this and do this manually. So I'll click on "+". Click on "Value", and since it's connected to everything else, we can select "retrieve_detections". So I'll click on that. Let's drill into it. Select "body". Select "routing". Now, scroll down until you see "sid".

Here's a common scenario that I often see if you introduce a new action or an application. Sometimes, you'll need to rerun the Playbook to actually generate the events. What I mean by that, is if you take a look at the bottom right, we currently see the results as "null". That is because we need to rerun the Playbook while being connected to the new application, or action. Which in our case, we just introduced the new action of the Isolate Sensor. 

So I'll scroll back up here. Let's go ahead and click on "Events". Select the first one. Click "Re-emit". Now select the User Prompt. I'll go ahead and select "Visit page". Select the recent one. I'll select "Yes" to isolate and this time it will feed it down this way.

We can already see that there's an error which makes sense since we didn't do any configurations to it, other than changing the URL. But, what I wanted to demo for you, is that if I now click on the plus button. Click on "Value". Select "retrieve_detections". Expand "body". Select "Routing". Finally, then "sid". At the bottom right, we now have our sensor ID.

So I'll leave that as is. If you take a look at the variable, we see a period. So let's remove that. The Method will be "POST". Content is "json" and it's looking for a credential "lima_charlie". So what this tells me is that we need to authenticate. If we take a look at the credentials, we need to connect it using a credential.

So what I'll do is open up a new tab. Let's click on "Credentials". I'll click on "New credential" at the top right. Let's see if there's a LimaCharlie credential. There's none here. That's alright. What we can do is go ahead and Google "LimaCharlie's API". In LimaCharlie's documentation, I'll click on the first link.

It says, simply issue an HTTP POST request towards this particular domain. It says here this makes user API Keys very powerful, but also riskier to manage, therefore, we recommend using organization API Keys whenever possible. So what we can do is use an organization key. You might ask, "How do I get the API key from LimaCharlie?" Well, if you head over to LimaCharlie. Click on Access Management and there is the "REST API".

Here we have the API details, the User-generated API Keys, we also have Service Manage API Keys, and Ingestion. So in LimaCharlie's documentation, they mentioned using the organization APIs key, whenever possible. Here it appears that there's already a JWT. We can just go ahead and copy that. Now let's try it out.

So if I were to select the Credentials. Click on "New credentials". Let's try "Text". For the name, I'll just type in "limacharlie". For the description, "LimaCharlie API. I'll paste in the value here. For the domains, I'll select "*.limacharlie.io". What this is doing, is essentially, ensuring that the credential can only be used towards this site and its subdomains.

Let's go ahead and hit "Save". Nice, so we have our credentials here and back over into our story, I'll go ahead and refresh this. Now, what we can do is click on "Connect". Select the credentials that we just created. Now our isolate sensor should have valid credentials. Let's go ahead and try it out.

Currently, if I go into my LimaCharlie and go into my sensors. If I were to isolate it, we would not see this as "Allowed". So let's go ahead and rerun this. So I'll just click on the first event on the User Prompt. Click on "Re-emit" and then that means it should be a success. We can look at LimaCharlie. Refresh. Awesome, so we got this as isolated.


If I go into my Vultr and click on "View console". I shouldn't be able to ping or anything. So let me go ahead and hit cntrl+alt+delete. I'll type "ping mydfir.com". So now we have network access. I'll go back into my server and type in "cls" to clear out the screen. Let me try pinging 'mydfir' again.

So we do have a reply here. Let's do an endless ping, how about that? So that way we know it's pinging. So let's do this from the start. I'll click on "Events". The first one. Click on "Re-emit". So we do get our alerts here. Now we should get a new email, like so.

Finally, I'll go into the "Visit page" of our User Prompt. Select the latest one. Now, it says "MyDFIR-SOAR-EDR-Project". It gives me all of the important information here, aswell as the detection link. I'll click on "Yes" to isolate. Before I click on "Submit", I'll take a look to see if it's still pinging. And it is.

So I'll click on "Submit". Let's close this window out. Now our isolation should work. If we go back over to our, we should start seeing "general failure". Awesome. Going back into LimaCharlie. Refresh it. Looking again, we still have it as "Isolated".

So let's click on "Isolate From Network". Now we're near the end. Looking at our workflow, once the user selects "Yes", LimaCharlie will then go and isolate the machine. Now the "isolating machine" section is completed. The last section will be that a message containing the isolation status, and the computer has been isolated. That gets sent over to Slack. Once it's done isolating, it sends a message.

So let's copy Slack. I'll paste it in. I'll connect LimaCharlie over to Slack. We already have the computer variable, so I don't need to select that. I'll type "The computer:" and "has been isolated, please investigate." I'll include the isolation status.

So I'll say "Isolation Status:". I'll click on the plus button. Click on "Value" and we want to click on "isolate_sensor". I'll hover over the bottom right. It doesn't really tell us if this was successful, or not. Based on the '200', we can assume that isolation was completed. 

Instead, let's see if there is a LimaCharlie template. So go into "Templates". I'll type "limacharlie". Now, find and click "Get Isolation Status". I'll connect "Isolate Sensor" over to "Get Isolation Status". I'll change my "sid". I'll click on the plus button. Select "Value". Select "retrieve_detections". Go into "body'. Go into "routing" and then go into "sid".

So that is good. The Method is "GET". It's using the credential of "lima_charlie". So we need to be careful of this. This is not the credential to use. To change that, we simply need to click on "Connect" under the Credentials. Then, select "limacharlie". This will automatically update this. So just make sure of that, just in case you accidentally overlook that.

Once you get the isolation status, we should be good. So let me go ahead and test this out. We'll use the latest "Isolate Sensor". I'll click on "Test". Clicking the "get_isolation_status", we see a status of '200'. Let's go into "body". And we see "is_isolated" is "false". That is the field that we want.

So click on the plus button where it says "Isolation Status:". I'll click on "Value". Select "get_isolation_status". Now if you take a look at the bottom right, we see it as "null". Remember what I said earlier? We need to rerun the playbook in that case. So before we do anything, let's rerun it.

I'll start from the User Prompt since we don't need to send a message to Slack or an email. So I'll click on "Events". Select the latest one. the first one click on Reit actually before I do that is it allowed yes it is okay and then I'll click on Reit so the sensor should be isolated let's take a look so that's isolated and then get isolation status and then it will send it over to slack so we take a look at slack wow okay we get a big isolation status so we need to change that but we do have the message of the computer my defer sore EDR has been isolated we're close I'll remove the get isolation status click on the plus button value I'll click on this get isolation status click on that again and it was under body I believe yeah so if you take a look at the results it says is isolated true so I'll click on body and I'll use the field value of is isolated because we want to know the status of the isolation and currently the bottom right it is set to true so Moment of Truth here I will go ahead and rejoin the network and what I'm going to do is just to make sure everything's nice and clean is just put a bunch of A's that way we can see the new message so from the beginning we click on events click on the first event Reit our workflow should now kick off we should get a slack message which contains the information related to the alert we should also get an email which yeah we get it right here and then from the user prompt let's go ahead and visit the page and we get it here isolate yes submit and I'm just going to go straight to slack look at that isolation status true the computer might dfir sore EDR has been isolated if we go into lima charlie refresh isolate it if we go into our server I haven't stopped this ping whoops let's say pingy youtube.com this time General failure that is amazing aming if you followed along congratulations on completing the sore EDR project this project was not easy and you should be extremely proud of yourself for tackling this we went over a ton of material and it can get pretty technical very quickly if you enjoyed this project I would highly recommend that you take a look at my other projects that I have such as the sock automation project and the active directory project if you haven't done those already if you're an aspiring blue teamer specifically within the security operations domain you'll love the sof course that I have created and again I'll leave the link down for you to check it out if you're interested it would mean a lot to me if you shared this project with other aspiring analysts who want to get some hands-on experience with that being said thank you so much for watching and I truly hope that this has helped some of you remember to stay curious and do things differently
