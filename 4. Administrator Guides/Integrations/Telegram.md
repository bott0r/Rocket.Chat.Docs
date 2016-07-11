Simple setup of a telegram Bot:

1.	Open telegram chat in your browser or mobile phone
2.	Add the user BotFather
3.	Click start
4.	Click /newbot
5.	Follow Instructions

![instructions](http://i.imgur.com/8y9SG49.jpg?1)

6.	Wait for the Token to be displayed referred to as [myauthorization-token]
7.	Generate Incoming Webhook in Rocketchat, follow instructions,  enable script, and paste the following
```javascript
class Script {

process_incoming_request({ request }) {
// https://api.telegram.org/file/bot185440081:AAGnLHwB4Aveq5pZCGisnc1onscG_rP4iwk/photo/file_157.jpg
//checks for sticker
if(request.content.message.sticker){
  arr1 = request.content.message.sticker
return {
content: {
  username: request.content.message.from.first_name,
  icon_url: '/avatar/' + request.content.message.from.first_name + '.jpg' ,
  text: ' '+ request.content.message.sticker.emoji  
  // attachments: request.content.message.sticker.emoji
 }
};
}
//checks for photo 
if(request.content.message.photo){
  arr2 = request.content.message.photo
  
return {
  
content: {
  username: request.content.message.from.first_name,
  icon_url: '/avatar/' + request.content.message.from.first_name + '.jpg' ,
  text:  'https://api.telegram.org/file/bot185440081:AAGnLHwB4Aveq5pZCGisnc1onscG_rP4iwk/'+arr2[0].file_path ,
  //+ 'request full image here [link](https://api.telegram.org/bot185440081:AAGnLHwB4Aveq5pZCGisnc1onscG_rP4iwk/getFile?file_id='+ arr2[2].file_id+')' 
  // request bigger image 'https://api.telegram.org/bot185440081:AAGnLHwB4Aveq5pZCGisnc1onscG_rP4iwk/getFile?file_id='+ arr2[2].file_id
 }
};

}

  
  
console.log(request.content);

return {
content: {
  username: request.content.message.from.first_name,
  icon_url: '/avatar/' + request.content.message.from.first_name + '.jpg' ,
  text: request.content.message.text + ' ', 
  // attachments: request.content.message.sticker.emoji
 }
};

return {
 error: {
   success: false,
   message: 'Error example'
 }
};
}
}
```
8.	Copy incoming webhook URL from rocketchat
9.	Change following URL with your token and Incoming webhookURL and excute in regular browser  
https://api.telegram.org/bot[myauthorization-token]/setwebhook?url=[Incoming_Webhook_Link_from_RocketChat]
10.	Receive message  {"ok":true,"result":true,"description":"Webhook succsessfully set"} (or similar)
11.	Test your incoming Webhook by sending a telegram message to the bot, it should be posted in the chan-nel/user you specified in the incoming webhook, check Rocketchat Console Log and write down Chat_id [chat-id]
12.	Create outgoing webhook and specify channel, add the following adepted url:
https://api.telegram.org/bot[myauthorization-token]/sendMessage?chat_id=[chat-id]
13.	Paste the script:
```javascript
class Script {
  prepare_outgoing_request({ request }) {
    
    let match;

    // Change the URL and method of the request
    match = request.data.user_name.match(/rocket.cat/);
    if (match) {
      return {
        // url: request.url + '&parse_mode=Markdown' + '&text=' + '*' + request.data.user_name+ '*: _' + request.data.text + '_',
        //no get method so nothing will happen avoid looping of messages
      }; 
    } else {
      return {
        url: request.url + '&parse_mode=HTML' + '&text=' + '<b>' + request.data.user_name+ '</b>: ' + request.data.text,
        method: 'GET'
      }; 
    }
  }
}
```
14.	Enable listening at the Bot with /privacy and to disable
![disableprivaceinbot](http://i.imgur.com/xSjdAAy.jpg?1)
15.	Add Bot to telegram group and utilize nice cross platform communication.


Cheers! 
(I am not a code wizard, which becomes apparent when reading the scripts, i am sure if someone digs into the telegram Bot API there are many more things possible) 

![final product](http://i.imgur.com/LqpqUC8.jpg?1)

edit: Sticker will be replaced with smilies
