ChameraVote
=======
ChameraVote is a solution for online votings.

Intro
-----------
This application was developped to allow dealing with online votings (for example NGO votings). 
Key feature is that it is possible to restrict list of users who can cast their vote, authorize them, but also keep their ballots anonymous to a certain level.
Ballot anonymization is implemented through RSA blind signatures.

Server
-----------
The server side is implemented with Python 3.
Server application requires a few python packages but it should run on any machine that supports Python 3.

Client
-----------
There are 2 client applications available (Web client - .html+.js and Desktop client - c#+WPF ).
Web client was made to run on any browser (tested on chrome and opera). 
Desktop client is made for Windows only.

| Feature         | Web client       | Desktop client  |
|-----------------|:----------------:|----------------:|
| Registration    | +                |+                |
| Logging in      | +                |+                |
| Casting votes   | +                |+                |
| Adding votings  | -                |+                |
| Browsing results| -                |+                |

Application is still under developpment.

Upcoming features:
  * equal features for web and desktop client
  * printing results to a document
  * password change (possibly with configurable smtp to send pwd reset codes, that would also require adding e-mail to the database)
  * localization
  * more admin tools


Below you may find some screenshots from desktop client.
![alt text](https://github.com/skowront/ChameraVote/blob/master/github/images/chameraVoteClient.jpg)


![alt text](https://github.com/skowront/ChameraVote/blob/master/github/images/chameraVoteClientResults.jpg)

API Documnentation:
Server is available at port 16403.
Commands are sent as strings, separator is ':'.
- command:getVotingById:{username:String}:{Token:String}:{Password:String}:{VotingId:Integer} returns {VotingId:Integer}:{Owner:string}:{Title:String}:{Anonymous:bool}:{MutuallyExclusive:bool}:{AllowUnregisteredUsers:bool}:{maxOptions:Integer}:{NumberOfVoteOptions:Integer}:{VoteOptions:[string]}:{NumberOfVoteResults}:{VoteResults:[String]}:{NumberOfVoteClients:Integer}:{VoteClients:[String]}
- command:getBallot:{votingId:Integer} returns {ballotIds:Integer}
- command:getSignedBallot:{username:String}:{token:String}:{password:String}:{votingId:Integer}:{mPrime:Integer} returns {sPrime:Integer}
- command:getTitle:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {voteTitle:String}
- command:getOptions:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {ret:[String]}
- command:getAnonymous:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {anonymous:bool}
- command:getMutuallyExclusive:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {mutuallyExclusive:bool}
- command:getOwner:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {owner:String}
- command:getClients:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {ret:[String]}
- command:getSignedClients:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {ret:[String]}
- command:getResults:{username:String}:{token:String}:{password:String}:{votingId:Integer} returns {ret:[String]}
- command:castVote:{username:String}:{token:String}:{password:String}:{votingId:Integer}:{Signature:String} returns {}
- command:login:{username:String}:{password:String} returns {}
- command:getUserVotings:{username:String}:{token:String}:{password:String} returns {response:String}:[{VotingId:Integer}:{Title:String}]
- commamd:register:{username:String}:{token:String}:{password:String} returns {token:String}
- commamd:addNewVoting:{username:String}:{token:String} returns {VotingId:Integer}
- commamd:removeVoting:{username:String}:{token:String}:{id:Integer} returns {}
- commamd:verify:{cardId:Integer}:{signature:String}:{VotingId:Integer} returns {}
Each response is also wrapped in status + value:
{OK}:{ResponseValue:String} or {NOK}:{ErrorCode:Integer}.
