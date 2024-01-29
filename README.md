
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

###### dtypes

        call_type : str ,value: audio|video
        buddy : str ,value: old buddy saved in contact list.
        dest_no : int
        your_extension : int

##### After that, go to AddLineHtml method and modify html accordingly to edit the htl elements for call progress section.You can append the final html to any part or section of your web page in that function accordingly.

### Here is the code sample of AddLineHtml function:


`    function AddLineHtml(lineObj, direction){
        var avatar = getPicture(lineObj.BuddyObj.identity);

        var html = "<table id=\"line-ui-"+ lineObj.LineNumber +"\" class=stream cellspacing=0 cellpadding=0>";
        html += "<tr><td class=\"streamSection highlightSection\" style=\"height: 85px;\">";

        // Close|Return|Back Button
        html += "<div style=\"float:left; margin:0px; padding:5px; height:38px; line-height:38px\">"
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-back\" onclick=\"CloseLine('"+ lineObj.LineNumber +"')\" class=roundButtons title=\""+ lang.back +"\"><i class=\"fa fa-chevron-left\"></i></button> ";
        html += "</div>"

        // Profile UI
        html += "<div class=contact style=\"cursor: unset; float: left;\">";
        html += "<div id=\"line-ui-"+ lineObj.LineNumber +"-LineIcon\" class=lineIcon>"+ lineObj.LineNumber +"</div>";
        html += "<div id=\"line-ui-"+ lineObj.LineNumber +"-DisplayLineNo\" class=contactNameText><i class=\"fa fa-phone\"></i> "+ lang.line +" "+ lineObj.LineNumber +"</div>";
        html += "<div class=presenceText style=\"max-width:150px\">"+ lineObj.DisplayNumber +"</div>";
        html += "</div>";

        // Audio Activity
        html += "<div style=\"float:right; line-height: 46px;\">";
        html += "<div  id=\"line-"+ lineObj.LineNumber +"-monitoring\" style=\"margin-right:10px\">";
        html += "<span style=\"vertical-align: middle\"><i class=\"fa fa-microphone\"></i></span> ";
        html += "<span class=meterContainer title=\""+ lang.microphone_levels +"\">";
        html += "<span id=\"line-"+ lineObj.LineNumber +"-Mic\" class=meterLevel style=\"height:0%\"></span>";
        html += "</span> ";
        html += "<span style=\"vertical-align: middle\"><i class=\"fa fa-volume-up\"></i></span> ";
        html += "<span class=meterContainer title=\""+ lang.speaker_levels +"\">";
        html += "<span id=\"line-"+ lineObj.LineNumber +"-Speaker\" class=meterLevel style=\"height:0%\"></span>";
        html += "</span> ";
        html += "</div>";
        
        html += "</div>";

        // Separator --------------------------------------------------------------------------
        html += "<div style=\"clear:both; height:0px\"></div>"

        // General Messages
        html += "<div id=\"line-"+ lineObj.LineNumber +"-timer\" class=CallTimer></div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-msg\" class=callStatus style=\"display:none\">...</div>";

        // Remote Audio Object
        html += "<div style=\"display:none;\">";
        html += "<audio id=\"line-"+ lineObj.LineNumber +"-remoteAudio\"></audio>";
        html += "</div>";

        html += "</td></tr>";

        // Next Has Dynamic Content
        html += "<tr><td id=\"line-"+ lineObj.LineNumber +"-call-fullscreen\" class=\"streamSection highlightSection\">"

        // Call Answer UI
        html += "<div id=\"line-"+ lineObj.LineNumber +"-AnswerCall\" style=\"display:none\">";
        html += "<div class=\"CallPictureUnderlay\" style=\"background-image: url('"+ avatar +"')\"></div>";
        html += "<div class=\"CallColorUnderlay\"></div>";
        html += "<div class=\"CallUi\">";
        html += "<div class=callingDisplayName>"+ lineObj.DisplayName +"</div>";
        html += "<div class=callingDisplayNumber>"+ lineObj.DisplayNumber +"</div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-in-avatar\" class=\"inCallAvatar\" style=\"background-image: url('"+ avatar +"')\"></div>";
        html += "<div class=answerCall>";
        html += "<button onclick=\"AnswerAudioCall('"+ lineObj.LineNumber +"')\" class=answerButton><i class=\"fa fa-phone\"></i> "+ lang.answer_call +"</button> ";
        if(EnableVideoCalling == true) {
            html += " <button id=\"line-"+ lineObj.LineNumber +"-answer-video\" onclick=\"AnswerVideoCall('"+ lineObj.LineNumber +"')\" class=answerButton><i class=\"fa fa-video-camera\"></i> "+ lang.answer_call_with_video +"</button> ";
        }
        html += " <button onclick=\"RejectCall('"+ lineObj.LineNumber +"')\" class=rejectButton><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.reject_call +"</button> ";
        // CRM
        html += "<div id=\"line-"+ lineObj.LineNumber +"-answer-crm-space\">"
        // Use this DIV for anything really. Call your own CRM, and have the results display here
        html += "</div>"; // crm
        html += "</div>"; //.answerCall

        html += "</div>"; //.CallUi
        html += "</div>"; //-AnswerCall

        // Dialing Out Progress
        html += "<div id=\"line-"+ lineObj.LineNumber +"-progress\" style=\"display:none\">";
        html += "<div class=\"CallPictureUnderlay\" style=\"background-image: url('"+ avatar +"')\"></div>";
        html += "<div class=\"CallColorUnderlay\"></div>";
        html += "<div class=\"CallUi\">";
        html += "<div class=callingDisplayName>"+ lineObj.DisplayName +"</div>";
        html += "<div class=callingDisplayNumber>"+ lineObj.DisplayNumber +"</div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-out-avatar\" class=\"inCallAvatar\" style=\"background-image: url('"+ avatar +"')\"></div>";
        html += "<div class=progressCall>"
        html += "<button onclick=\"cancelSession('"+ lineObj.LineNumber +"')\" class=rejectButton><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.cancel +"</button>"
        html += " <button id=\"line-"+ lineObj.LineNumber +"-early-dtmf\" onclick=\"ShowDtmfMenu('"+ lineObj.LineNumber +"')\" style=\"display:none\"><i class=\"fa fa-keyboard-o\"></i> "+ lang.send_dtmf +"</button>"
        html += "</div>"; //.progressCall
        html += "</div>"; //.CallUi
        html += "</div>"; // -progress

        // Active Call UI
        html += "<div id=\"line-"+ lineObj.LineNumber +"-ActiveCall\" class=cleanScroller style=\"display:none; position: absolute; top: 0px; left: 0px; height: 100%; width: 100%;\">";

        // Audio or Video Call (gets changed with InCallControls)
        html += "<div id=\"line-"+ lineObj.LineNumber +"-AudioOrVideoCall\" style=\"height:100%\">";

        // Audio Call UI
        html += "<div id=\"line-"+ lineObj.LineNumber +"-AudioCall\" style=\"height:100%; display:none\">";
        html += "<div class=\"CallPictureUnderlay\" style=\"background-image: url('"+ avatar +"')\"></div>";
        html += "<div class=\"CallColorUnderlay\"></div>";
        html += "<div class=\"CallUi\">";
        html += "<div class=callingDisplayName>"+ lineObj.DisplayName +"</div>";
        html += "<div class=callingDisplayNumber>"+ lineObj.DisplayNumber +"</div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-session-avatar\" class=\"inCallAvatar\" style=\"background-image: url('"+ avatar +"')\"></div>";

        // Call Transfer
        html += "<div id=\"line-"+ lineObj.LineNumber +"-Transfer\" style=\"text-align: center; line-height:40px; display:none\">";
        html += "<div style=\"margin-top:10px\">";
        html += "<span class=searchClean><input id=\"line-"+ lineObj.LineNumber +"-txt-FindTransferBuddy\" oninput=\"QuickFindBuddy(this,'"+ lineObj.LineNumber +"')\" onkeydown=\"transferOnkeydown(event, this, '"+ lineObj.LineNumber +"')\" type=text autocomplete=none style=\"width:150px;\" autocomplete=none placeholder=\""+ lang.search_or_enter_number +"\"></span>";
        html += "<br>"
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-blind-transfer\" onclick=\"BlindTransfer('"+ lineObj.LineNumber +"')\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i> "+ lang.blind_transfer +"</button>"
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-attended-transfer\" onclick=\"AttendedTransfer('"+ lineObj.LineNumber +"')\"><i class=\"fa fa-reply-all\" style=\"transform: rotateY(180deg)\"></i> "+ lang.attended_transfer +"</button>";
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-complete-attended-transfer\" style=\"display:none\"><i class=\"fa fa-reply-all\" style=\"transform: rotateY(180deg)\"></i> "+ lang.complete_transfer +"</button>";
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-cancel-attended-transfer\" style=\"display:none\"><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.cancel_transfer +"</button>";
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-terminate-attended-transfer\" style=\"display:none\"><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.end_transfer_call +"</button>";
        html += "</div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-transfer-status\" class=callStatus style=\"margin-top:10px; display:none\">...</div>";
        html += "<audio id=\"line-"+ lineObj.LineNumber +"-transfer-remoteAudio\" style=\"display:none\"></audio>";
        html += "</div>"; //-Transfer

        // Call Conference
        html += "<div id=\"line-"+ lineObj.LineNumber +"-Conference\" style=\"text-align: center; line-height:40px; display:none\">";
        html += "<div style=\"margin-top:10px\">";
        html += "<span class=searchClean><input id=\"line-"+ lineObj.LineNumber +"-txt-FindConferenceBuddy\" oninput=\"QuickFindBuddy(this,'"+ lineObj.LineNumber +"')\" onkeydown=\"conferenceOnkeydown(event, this, '"+ lineObj.LineNumber +"')\" type=text autocomplete=none style=\"width:150px;\" autocomplete=none placeholder=\""+ lang.search_or_enter_number +"\"></span>";
        html += "<br>"
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-conference-dial\" onclick=\"ConferenceDial('"+ lineObj.LineNumber +"')\"><i class=\"fa fa-phone\"></i> "+ lang.call +"</button>";
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-cancel-conference-dial\" style=\"display:none\"><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.cancel_call +"</button>";
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-join-conference-call\" style=\"display:none\"><i class=\"fa fa-users\"></i> "+ lang.join_conference_call +"</button>";
        html += " <button id=\"line-"+ lineObj.LineNumber +"-btn-terminate-conference-call\" style=\"display:none\"><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.end_conference_call +"</button>";
        html += "</div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-conference-status\" class=callStatus style=\"margin-top:10px; display:none\">...</div>";
        html += "<audio id=\"line-"+ lineObj.LineNumber +"-conference-remoteAudio\" style=\"display:none\"></audio>";
        html += "</div>"; //-Conference

        // CRM
        html += "<div id=\"line-"+ lineObj.LineNumber +"-active-audio-call-crm-space\">"
        // Use this DIV for anything really. Call your own CRM, and have the results display here
        html += "</div>"; // crm

        html += "</div>"; //.CallUi
        html += "</div>"; //AudioCall

        // Video Call UI
        html += "<div id=\"line-"+ lineObj.LineNumber +"-VideoCall\" style=\"height:100%; display:none\">";
        // Video Preview
        html += "<div id=\"line-"+ lineObj.LineNumber +"-preview-container\" class=\"PreviewContainer cleanScroller\">";
        html += "<video id=\"line-"+ lineObj.LineNumber +"-localVideo\" muted playsinline></video>"; // Default Display
        html += "</div>";

        // Stage
        html += "<div id=\"line-"+ lineObj.LineNumber +"-stage-container\" class=StageContainer>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-remote-videos\" class=VideosContainer></div>";
        html += "<div id=\"line-"+ lineObj.LineNumber +"-scratchpad-container\" class=ScratchpadContainer style=\"display:none\"></div>";
        html += "<video id=\"line-"+ lineObj.LineNumber +"-sharevideo\" controls muted playsinline style=\"display:none; object-fit: contain; width: 100%;\"></video>";
        html += "</div>";
        html += "</div>"; //-VideoCall
        html += "</div>"; //-AudioOrVideoCall

        // In Call Control
        // ===============
        html += "<div class=CallControlContainer>";
        html += "<div class=CallControl>";
        html += "<div><button id=\"line-"+ lineObj.LineNumber +"-btn-ControlToggle\" onclick=\"ToggleMoreButtons('"+ lineObj.LineNumber +"')\" style=\"font-size:24px; width:80px\"><i class=\"fa fa-chevron-up\"></i></button></div>";
        // Visible Row
        html += "<div>";
        // Mute
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Mute\" onclick=\"MuteSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.mute +"\"><i class=\"fa fa-microphone-slash\"></i></button>";
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Unmute\" onclick=\"UnmuteSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.unmute +"\" style=\"color: red; display:none\"><i class=\"fa fa-microphone\"></i></button>";
        // Hold
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Hold\" onclick=\"holdSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\"  title=\""+ lang.hold_call +"\"><i class=\"fa fa-pause-circle\"></i></button>";
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Unhold\" onclick=\"unholdSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.resume_call +"\" style=\"color: red; display:none\"><i class=\"fa fa-play-circle\"></i></button>";

        if(direction == "outbound"){
            // DTMF
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-ShowDtmf\" onclick=\"ShowDtmfMenu('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.send_dtmf +"\"><i class=\"fa fa-keyboard-o\"></i></button>";
        } else {
            // Transfer (Audio Only)
            if(EnableTransfer){
                html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Transfer\" onclick=\"StartTransferSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.transfer_call +"\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i></button>";
                html += "<button id=\"line-"+ lineObj.LineNumber+"-btn-CancelTransfer\" onclick=\"CancelTransferSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.cancel_transfer +"\" style=\"color: red; display:none\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i></button>";
            }
        }

        // Expand UI (Video Only)
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-expand\" onclick=\"ExpandVideoArea('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\"><i class=\"fa fa-expand\"></i></button>";
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-restore\" onclick=\"RestoreVideoArea('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" style=\"display:none\"><i class=\"fa fa-compress\"></i></button>";
        // End Call
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-End\" onclick=\"endSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons hangupButton\" title=\""+ lang.end_call +"\"><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i></button>";
        html += "</div>";
        // Row two (Hidden By Default)
        html += "<div id=\"line-"+ lineObj.LineNumber +"-btn-more\" style=\"display:none\">";
        // Record
        if(typeof MediaRecorder != "undefined" && (CallRecordingPolicy == "allow" || CallRecordingPolicy == "enabled")){
            // Safari: must enable in Develop > Experimental Features > MediaRecorder
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-start-recording\" onclick=\"StartRecording('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.start_call_recording +"\"><i class=\"fa fa-dot-circle-o\"></i></button>";
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-stop-recording\" onclick=\"StopRecording('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.stop_call_recording +"\" style=\"color: red; display:none\"><i class=\"fa fa-circle\"></i></button>";
        }
        // Conference
        if(EnableConference){
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Conference\" onclick=\"StartConferenceCall('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.conference_call +"\"><i class=\"fa fa-users\"></i></button>";
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-CancelConference\" onclick=\"CancelConference('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.cancel_conference +"\" style=\"color: red; display:none\"><i class=\"fa fa-users\"></i></button>";
        }
        if(direction == "outbound"){
            // Transfer (Audio Only)
            if(EnableTransfer){
                html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-Transfer\" onclick=\"StartTransferSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.transfer_call +"\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i></button>";
                html += "<button id=\"line-"+ lineObj.LineNumber+"-btn-CancelTransfer\" onclick=\"CancelTransferSession('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.cancel_transfer +"\" style=\"color: red; display:none\"><i class=\"fa fa-reply\" style=\"transform: rotateY(180deg)\"></i></button>";
            }
        } else {
            // DTMF
            html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-ShowDtmf\" onclick=\"ShowDtmfMenu('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.send_dtmf +"\"><i class=\"fa fa-keyboard-o\"></i></button>";
        }
        // Settings
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-settings\" onclick=\"ChangeSettings('"+ lineObj.LineNumber +"', this)\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.device_settings +"\"><i class=\"fa fa-volume-up\"></i></button>";
        // Present
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-present-src\" onclick=\"ShowPresentMenu(this, '"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.camera +"\"><i class=\"fa fa-video-camera\"></i></button>";
        // Call Stats
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-ShowCallStats\" onclick=\"ShowCallStats('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.call_stats +"\"><i class=\"fa fa-area-chart\"></i></button>";
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-HideCallStats\" onclick=\"HideCallStats('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.call_stats +"\" style=\"color: red; display:none\"><i class=\"fa fa-area-chart\"></i></button>";
        // Call Timeline
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-ShowTimeline\" onclick=\"ShowCallTimeline('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.activity_timeline +"\"><i class=\"fa fa-list-ul\"></i></button>";
        html += "<button id=\"line-"+ lineObj.LineNumber +"-btn-HideTimeline\" onclick=\"HideCallTimeline('"+ lineObj.LineNumber +"')\" class=\"roundButtons dialButtons inCallButtons\" title=\""+ lang.activity_timeline +"\" style=\"color: red; display:none\"><i class=\"fa fa-list-ul\"></i></button>";

        html += "</div>"; // More Buttons Row
        html += "</div>"; // .CallControl
        html += "</div>"; // .CallControlContainer

        // Screens - Note: They cannot occupy the same space.
        
        // Call Stats
        html += "<div id=\"line-"+ lineObj.LineNumber +"-AudioStats\" class=\"audioStats cleanScroller\" style=\"display:none\">";
        html += "<div>";
        html += "<div>"+ lang.send_statistics +"</div>";
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioSendBitRate\" class=audioGraph></canvas></div>"
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioSendPacketRate\" class=audioGraph></canvas></div>";
        html += "</div>";
        html += "<div>";
        html += "<div>"+ lang.receive_statistics +"</div>";
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioReceiveBitRate\" class=audioGraph></canvas></div>";
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioReceivePacketRate\" class=audioGraph></canvas></div>";
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioReceivePacketLoss\" class=audioGraph></canvas></div>";
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioReceiveJitter\" class=audioGraph></canvas></div>";
        html += "<div style=\"position: relative; margin: auto; height: 160px; width: 100%;\"><canvas id=\"line-"+ lineObj.LineNumber +"-AudioReceiveLevels\" class=audioGraph></canvas></div>";
        html += "</div>";
        html += "</div>";

        // Call Timeline
        html += "<div id=\"line-"+ lineObj.LineNumber +"-CallDetails\" class=\"callTimeline cleanScroller\" style=\"display:none\">";
        // In Call Activity
        html += "</div>";

        html += "</div>"; // Active Call UI


        html += "</td></tr>";
        html += "</table>";

        $("#callProgModalContent").html(html);
        $("#callProgModal").modal("show");

        $("#line-"+ lineObj.LineNumber +"-AudioOrVideoCall").on("click", function(){
            RestoreCallControls(lineObj.LineNumber);
        });
    }`




#### You can see the coments describing all the elements' functionalities respectively.You can add eventListener in jS to call these functions.
#### For Example:

   """// Call Answer UI
    html += "<div id=\"line-"+ lineObj.LineNumber +"-AnswerCall\" style=\"display:none\">";
    html += "<div class=\"CallPictureUnderlay\" style=\"background-image: url('"+ avatar +"')\"></div>";
    html += "<div class=\"CallColorUnderlay\"></div>";
    html += "<div class=\"CallUi\">";
    html += "<div class=callingDisplayName>"+ lineObj.DisplayName +"</div>";
    html += "<div class=callingDisplayNumber>"+ lineObj.DisplayNumber +"</div>";
    html += "<div id=\"line-"+ lineObj.LineNumber +"-in-avatar\" class=\"inCallAvatar\" style=\"background-image: url('"+ avatar +"')\"></div>";
    html += "<div class=answerCall>";
    html += "<button onclick=\"AnswerAudioCall('"+ lineObj.LineNumber +"')\" class=answerButton><i class=\"fa fa-phone\"></i> "+ lang.answer_call +"</button> ";
    if(EnableVideoCalling == true) {
        html += " <button id=\"line-"+ lineObj.LineNumber +"-answer-video\" onclick=\"AnswerVideoCall('"+ lineObj.LineNumber +"')\" class=answerButton><i class=\"fa fa-video-camera\"></i> "+ lang.answer_call_with_video +"</button> ";
    }
    html += " <button onclick=\"RejectCall('"+ lineObj.LineNumber +"')\" class=rejectButton><i class=\"fa fa-phone\" style=\"transform: rotate(135deg);\"></i> "+ lang.reject_call +"</button> ";
    // CRM
    html += "<div id=\"line-"+ lineObj.LineNumber +"-answer-crm-space\">"
    // Use this DIV for anything really. Call your own CRM, and have the results display here
    html += "</div>"; // crm
    html += "</div>"; //.answerCall

    html += "</div>"; //.CallUi
    html += "</div>"; //-AnswerCall"""

##### The above code snippet is a part if AddLineHtml function and is briefly describing all the html elements in Call answering UI that can be modified but make sure not to change id,classes, and functions added on click event listener.
