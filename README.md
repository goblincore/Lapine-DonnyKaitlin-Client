## Bunny Babble ♥

**Bunny Babble** is a simple learning app that helps you learn Lapine, the language of the bunnies. Lapine was initially created in 1972 by the author Richard Adams for his novel 'Watership Down'. Through this app, you will learn vocabulary through the use of a spaced-repetition algorithm, which adjusts the frequency of individual vocabulary words based on your performance to help you learn as efficiently as possible. 

<img src="https://github.com/thinkful-ei22/Lapine-DonnyKaitlin-Client/blob/master/screenshots/app2.png" width=100%>

## Deployed Version

The deployed version of our app can be found here:

**https://bunneh-client.herokuapp.com**

Server-side repo:

**https://github.com/thinkful-ei22/Lapine-DonnyKaitlin-Server**

## Algorithm Implementation

The spaced-repetition algorithm is implemented server-side via a singly linked list data structure. There are two main parts to this implementation: 

1) In the User schema where we define our node for our linked list - in this case, our questions are an array of objects that each have a "next" property that acts as a pointer to the next node (or question in our case). Each question also has a property we have called "mValue" that will be for tracking and adjusting the frequency of a word's appearance. We also have a property called head that keeps track of the current head of our linked list. 

2)  When the user submits their answer, the answer connects to the backend API and hits a POST endpoint which then takes the submitted answer, compares it with the correct answer, adjusts the mValue and sets the head of our linked list to the the next value of our current node - which if you recall, each node is a question in our case.

A detailed and commented walk-through of implemented code is found below:

```
      //Local variables having to do with the position
      let currIndex = user.head;
      let question = user.questions[currIndex];

      //Local variables having to do with the accuracy tracking
      let answer = user.questions[currIndex].question.englishWord;
      let correctGuess = true;

      //Increment attempt counter
      user.questions[currIndex].attempts += 1;

      // Compare our user input (guess) with our correct answer
      // console.log('answer',answer,'guess',guess); 
      if(answer === guess.replace(/\s+/g, '').toLowerCase()){
        correctGuess = true;
        user.questions[currIndex].correctAnswers +=1;
        user.questions[currIndex].mValue *= 2;
      }
      else{
        correctGuess = false;
        user.questions[currIndex].mValue = 1;
      }

      //Set head to the value of next pointer (unless it's the end)
      //This is what changes the word being displayed -
      //every time you send a post request, the head is set to the next item
      if (user.head === null) {
        user.head = 0;
      } else {
        user.head = question.next;
      }

      //Move the question back based on the mValue calculated above
      //temp variable that stores a copy of our current node and a counter
      let currNode = question;
      let counter = 0;

      //Loop until we reach our mValue (our new position)
      //Each iteration, move to the next node 
      while(counter !== question.mValue){
        if (currNode.mValue > user.questions.length) {
          currNode.mValue = user.questions.length - 1;
       
        }
        (currNode !== null) ?
          currNode = user.questions[currNode.next] : 
          currNode = user.questions[user.head];
        counter++;
      }
      
      //When the new position is reached, 
      //set the current nodes's next pointer value to the temporary node's next value
      //And the temporary next pointer's value to the original node's head
      //this effectively inserts the original node at a new position 
      // M spaces away from where it was originally
    
      question.next = currNode.next;
      currNode.next = currIndex;
```

## Screenshots

`Live App`

<img src="https://github.com/thinkful-ei22/Lapine-DonnyKaitlin-Client/blob/master/screenshots/app1.png" width=100%>
<img src="https://github.com/thinkful-ei22/Lapine-DonnyKaitlin-Client/blob/master/screenshots/app4.png" width=100%>
<img src="https://github.com/thinkful-ei22/Lapine-DonnyKaitlin-Client/blob/master/screenshots/app3.png" width=100%>



## Tech Stack

`Frontend`

* React (16.2)
* React Router
* Redux & Redux Form
* React FullPage
* CSS
* Media Queries

`Backend`

* Node
* Express
* Mocha
* Chai
* Passport & JWTs

`Data Persistence`

* MongoDB

`Hosting`

* Heroku

## Future Plans

Later on, we will expand the app's vocabulary by adding more words to the database. Addtionally, we plan to implement a 'leaderboard' system, where the top scoring users will be displayed for all to see.
