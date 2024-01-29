# Asterisk Web Phone Documentation:
## Start by creating user agent to register your web phone with asterisk server:

### jquery:

        `$("#id_of_registration_button").on("click",function(e){
        e.preventDefault();
        try{
        localDB.setItem("wssServer", $("#input_id_for_server").val());
        localDB.setItem("WebSocketPort",$("#id_for_websocket_port_input").val());
        localDB.setItem("ServerPath", $("#id_for_websocket path input").val());
        localDB.setItem("profileName", $("#name").val());
        localDB.setItem("SipDomain",$("#domain").val());
        localDB.setItem("SipUsername", $("#username").val());
        localDB.setItem("SipPassword", $("#password").val());
        setTimeout(function(){
            console.log("Resgiter request sent.");
            CreateUserAgent(localDB.wssServer,localDB.SipDomain,localDB.WebSocketPort,localDB.ServerPath);
        }, 3000);
        }
        catch(err){
        
            setTimeout(function(){
            console.log("Resgiter request sent.");
            CreateUserAgent(localDB.wssServer,localDB.SipDomain,localDB.WebSocketPort,localDB.SipUsername,localDB.SipPassword,localDB.ServerPath);
        }, 3000);
        }
        }); `

### Vanilla JS:

        `document.getElementById("id_of_registration_button").addEventListener("click", function(e) {
        e.preventDefault();
        try{
        localDB.setItem("wssServer", document.getElementById("server_input_id").value);
        localDB.setItem("WebSocketPort", document.getElementById("websocket_port_input_id").value);
        localDB.setItem("ServerPath", document.getElementById("websocket_path_input_id").value);
        localDB.setItem("profileName", document.getElementById("profilename_input_id").value);
        localDB.setItem("SipDomain", document.getElementById("sipdomain_input_id").value);
        localDB.setItem("SipUsername", document.getElementById("username_input_id").value);
        setTimeout(function(){
            console.log("Resgiter request sent.");
            CreateUserAgent(localDB.wssServer,localDB.SipDomain,localDB.WebSocketPort,localDB.ServerPath);
        }, 3000);
        }
        catch(err){
        
            setTimeout(function(){
            console.log("Resgiter request sent.");
            CreateUserAgent(localDB.wssServer,localDB.SipDomain,localDB.WebSocketPort,localDB.SipUsername,localDB.SipPassword,localDB.ServerPath);
        }, 3000);
        }
        }); `

##### dtypes:

        wssServer : str
        SipDomain : str
        WebSocketPort : int
        SipUsername : str
        SipPassword : str
        ServerPath : str

###### CreateUserAgent is a primary function for connecting browser phone with an asterisk server.It is accepting 6 primary arguments wssServer,SipDomain,WebSocketPort,SipUsername,SipPassword snd ServerPath respectively.Other variables being used in the function are defined at the top of phone.js.You can(additionally) add them as arguments and pass custom values for them.

##### Note: For development purposes https connection with relative SipDomain must be established to overcome registration related errors.Just visit the following url and allow traffic: https://SipDomain:WebSocketPort

## Calling:

###### Once, you get registered, next step is to call another user using your webphone.The following code snippet can be used to do so:

        function dial([call_type,buddy,dest_no,your_extension]){
        DialByLine(call_type,buddy,dest_no,your_extension);
        }
        $("#call").on('click',function(){
            your_extension = 10 // may vary for different users depending on the asterisk side configurations.
            PreloadAudioFiles();
            dest_no = $("#output").val();
            if(dest_no != "" && dest_no != null){
                    last_call =['audio',"",dest_no,your_extension]
                    dial(last_call);
            }
            else{
            console.error("Please enter number to dial.");
            }
                });
                
###### dial() is a custom and generic function made for both audio/video calling and DialByLine is called in it.

###### dtypes

        call_type : str ,value: audio|video
        buddy : str ,value: old buddy saved in contact list.
        dest_no : int
        your_extension : int

##### After that, go to AddLineHtml() method and modify html accordingly to edit the html elements for call progress section.You can append the final html to any part or section of your web page in that function accordingly.

### Call Answer UI

#### You can see the coments describing all the elements' functionalities respectively.You can add eventListener in jS to call these functions.
#### For Example:

   `// Call Answer UI
    html += "<div id=\"line-"+ lineObj.LineNumber +"-AnswerCall\" style=\"display:none\">";
    html += "<div class=\"CallPictureUnderlay\" style=\"background-image: url('"+ avatar +"')\"></div>";
    html += "<div class=\"CallColorUnderlay\"></div>";
    html += "<div class=\"CallUi\">";
    html += "<div class=callingDisplayName>"+ lineObj.DisplayName +"</div>";
    html += "<div class=callingDisplayNumber>"+ lineObj.DisplayNumber +"</div>";
    html += "<div id=\"line-"+ lineObj.LineNumber +"-in-avatar\" class=\"inCallAvatar\" style=\"background-image: url('"+ avatar +"')\">        </div>";
    html += "<div class=answerCall>";
    html += "<button onclick=\"AnswerAudioCall('"+ lineObj.LineNumber +"')\" class=answerButton><i class=\"fa fa-phone\"></i> "+                 lang.answer_call +"</button> ";
    if(EnableVideoCalling == true) {
        html += " <button id=\"line-"+ lineObj.LineNumber +"-answer-video\" onclick=\"AnswerVideoCall('"+ lineObj.LineNumber +"')\"          class=answerButton><i class=\"fa fa-video-camera\"></i> "+ lang.answer_call_with_video +"</button> ";
    }
    html += " <button onclick=\"RejectCall('"+ lineObj.LineNumber +"')\" class=rejectButton><i class=\"fa fa-phone\" style=\"transform:         rotate(135deg);\"></i> "+ lang.reject_call +"</button> ";
    // CRM
    html += "<div id=\"line-"+ lineObj.LineNumber +"-answer-crm-space\">"
    // Use this DIV for anything really. Call your own CRM, and have the results display here
    html += "</div>"; // crm
    html += "</div>"; //.answerCall
    html += "</div>"; //.CallUi
    html += "</div>"; //-AnswerCall`

##### The above code snippet is a part of AddLineHtml function and is briefly describing all the html elements in Call answering UI that can be modified but make sure not to change id,classes, and functions added on click event listener.

#### Now, in the case another user calls you. You can see in the above code snippet the following line;

`    html += "<button onclick=\"AnswerAudioCall('"+ lineObj.LineNumber +"')\" class=answerButton><i class=\"fa fa-phone\"></i> "+ lang.answer_call +"</button> ";`

##### This is the call answering button you can add any html element instead with class = answerButton and can answer the call using the following function \"AnswerAudioCall('"+ lineObj.LineNumber +"')\". In the above example, this function is called on click event of html button with class 'answerButton'.

### Audio Call UI

##### Similarly, here is the part of AddLineHtml() function describing the ui for an audio call progress. You can modify it the same way explained in the above section.

`// Audio Call UI
    html += "<div id=\"line-"+ lineObj.LineNumber +"-AudioCall\" style=\"height:100%; display:none\">";
    html += "<div class=\"CallPictureUnderlay\" style=\"background-image: url('"+ avatar +"')\"></div>";
    html += "<div class=\"CallColorUnderlay\"></div>";
    html += "<div class=\"CallUi\">";
    html += "<div class=callingDisplayName>"+ lineObj.DisplayName +"</div>";
    html += "<div class=callingDisplayNumber>"+ lineObj.DisplayNumber +"</div>";
    html += "<div id=\"line-"+ lineObj.LineNumber +"-session-avatar\" class=\"inCallAvatar\" style=\"background-image: url('"+ avatar +"')\"></div>";
`

### Mute/Unmute/Hold:

##### Here is a code snippet again from AddLineHtml() function appending mute/unmute button to custom ui:

`// Mute
    html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Mute\" onclick=\"MuteSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.mute +"\"><i class=\"fa fa-microphone-slash\"></i></button>";
    html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Unmute\" onclick=\"UnmuteSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.unmute +"\" style=\"color: red; display:none\"><i class=\"fa fa-microphone\"></i></button>";`

##### Similarly, for hold button:

`    // Hold
    html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Hold\" onclick=\"holdSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\"  title=\""+ lang.hold_call +"\"><i class=\"fa fa-pause-circle\"></i></button>";
    html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Unhold\" onclick=\"unholdSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.resume_call +"\" style=\"color: red; display:none\"><i class=\"fa fa-play-circle\"></i></button>";`

### DTMF/ Call Transfer:

 ##### DTMF has been an interactive way to get response as signals from user. Here is a code snippet explaining the dtmf ui.
 `        // DTMF
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-ShowDtmf\" onclick=\"ShowDtmfMenu('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.send_dtmf +"\"><i class=\"fa fa-keyboard-o\"></i></button>";`

##### Similarly for transfering call to another agent/user , transfer button is used:

`        // Transfer (Audio Only)
        if(EnableTransfer){
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Transfer\" onclick=\"StartTransferSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.transfer_call +"\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i></button>";
            html += "<button id=\"line-"+ lineObj.LineNumber+"-btn-CancelTransfer\" onclick=\"CancelTransferSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.cancel_transfer +"\" style=\"color: red; display:none\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i></button>";
        }
    }`


##### Finally, after modifying the ui as per usage, you can append all the html to your desired html element at the end of function like this:

   ` document.getElementByID("element_id").innerHTML = html;`

##### On appending the following html you can see and modify all the elements defined in AddLineHtml() function.
