# Custom Command for YAGPDB Discord Bot
# Add a tag to the beginning of a server member's name
# Note: Trigger type must be set to Regex and trigger set to /d
# Note: This only searchs for messages containing numbers, the trigger must be adjusted for tags containing letters 

# Discord Nickname length is limited by 27 charaters, returns a message when this limit is hit
{{if gt (len .Member.Nick) 27}}
**Your nickname is currently longer than 27 chars and a tag cannot be added. Please request a shorter nickname**

{{else}}

# Define variable that scans message content input for numbers
# Note: Regex is used here to pull all digits in the message, if you want to pull more then digits you must change the regex 
{{$msg := reFindAll `\d+` .Message.Content}}

# Limits the tag contents to 4 digits and returns a message upon input of 5 or more digits 
{{if gt (len .Message.Content) 4}}
**Please Enter A Server Number**

{{else}}

# Tells bot to edit member's nickname, appending the message contents as the tag in front of the member's global discord name
{{editNickname (print  $msg " " .User.Globalname)}}

#Reacts to the trigger message with a emoji once tag change is complete
{{addMessageReactions nil $.Message.ID ":white_check_mark:"}}

#Close if else statements
{{end}}
{{end}}
