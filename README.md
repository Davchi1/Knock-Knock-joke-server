make use of C sockets to implement a simple network service: a knock 
knock joke serve

1. Background - Knock knock jokes
 The basic format of every knock knock joke is:
Knock, knock.
Who's there?
<setup line>.
<setup line>, who?
<punch line><punctuation>
<expression of annoyance and/or disgust or surprise><punctuation>
 The conceit of the format is apparently the taking advantage of the brief but plausible confusion of 
someone answering a door when an odd 'name' is given as an answer when a name is expected. By 
way of example:
Knock, knock.
Who's there?
Who.
Who, who?
I didn't know you were an owl!
Ugh.
Or:
Knock, knock.
Who's there?
Dijkstra.
Dijkstra, who?
That path was taking too long, so I let myself in.
Ahh!
2.a Implementation - KKJ Application Protocol
 Given the strict structure of knock knock jokes, the KKJ application protocol consists of the below.
 Notes and symbols:
all messages are character sequences, but are not sent as strings.
all messages are constructed using the KKJ Message Format (below).
values in double quotes (“ “) are character literals and must appear
 note this does not mean they are C strings, but a character list/array/sequence
 note this means double quotes are only tags, they are not data to be sent
values in angle brackets (< >) are variable, but required.
important events that are not messages are enclosed in parentheses ( ( ) ).
<expression of A/D/S> is an expression of annoyance, disgust or surprise
Server Client
(connect to server)
(begin read)
 (receive client connection)
 (send initial to client:)
 “Knock, knock.”
(receive initial message)
(send response to server:)
 “Who's there?”
 (recv response)
 (send setup to client:)
 <setup line>”.” (receive setup line)
(send response to server:)
 <setup line>”, who?”
 (recv response)
 (send punch to client:)
 <punch line><punct.>
(receive punch line)
(send response to server:)
 <expression of A/D/S><punct.>
 (recv expression of A/D/S)
 (close/free)
(close/free/return)
Errors and Exceptions:
 If ever a message is sent that does not match the requirements of the protocol, the side receiving the
bad message should not send the next message but instead respond with an error message (see the 
KKJ messaging format below) indicating the error type and should shut down their context gracefully.}

Error codes:
M0CT - message 0 content was not correct (i.e. should be “Knock, knock.”)
 M0LN - message 0 length value was incorrect (i.e. should be 13 characters long)
 M0FT - message 0 format was broken (did not include a message type, missing or too many '|')
M1CT - message 1 content was not correct (i.e. should be “Who's there?”)
M1LN - message 1 length value was incorrect (i.e. should be 12 characters long)
 M1FT - message 1 format was broken (did not include a message type, missing or too many '|')
M2CT - message 3 content was not correct (i.e. missing punctuation)
M2LN - message 2 length value was incorrect (i.e. should be the length of the message)
 M2FT - message 2 format was broken (did not include a message type, missing or too many '|')
M3CT - message 3 content was not correct
 (i.e. should contain message 2 with “, who?” tacked on)
M3LN - message 3 length value was incorrect (i.e. should be M2 length plus six)
 M3FT - message 3 format was broken (did not include a message type, missing or too many '|')
M4CT - message 4 content was not correct (i.e. missing punctuation)
M4LN - message 4 length value was incorrect (i.e. should be the length of the message)
 M4FT - message 4 format was broken (did not include a message type, missing or too many '|')
M5CT - message 5 content was not correct (i.e. missing punctuation)
M5LN - message 5 length value was incorrect (i.e. should be the length of the message)
 M5FT - message 5 format was broken (did not include a message type, missing or too many '|'
