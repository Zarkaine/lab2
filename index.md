## This is lab report #2

# Part 1: StringServer
First I had to create the server. I did this with the method given before for numberServer. I created an int called port, and called Server.start(port, new StringServerHandler()); StringServerHandler implements URLHandler. It has 3 instance variables, 2 arrayLists and one String. StringServerHandler has a single method, handleRequest. This method returns a String, and takes a URI as the argument. If the user adds "/add-message?s=<string>" to the url, it will then display the string, as shown below.
 
![one add](https://imgur.com/ZcMZImS.png)
 
I have an if statment checking if /add-message is in the url, then adds the string following the s to the arrayList. Then it copies it to the string, and returns the string. 

![two adds](https://imgur.com/1BpXJcK.png)
 
 This is the second message. After using /add-message a second time, it displays it & the previous messages. I originally had an issue where it would re-add previous add-messages to the string. So if you called "first", "second", and "third", it would display "first first second first second third". I resolved this by adding an if statment, so the each item from the arraylist would only be added once. There may have been a simpler/better way to do this, but I have a migraine & Im definitely out of time.
  
  
The relevant arguments for the method handleRequest was a URI
  
  
No values got changed because each time add-message was called, if parameter[0] was "s", then parameter[1] was added to the arrayList. Then to the string, and the string was returned. The only value that would've been changed is the string that comes after /add-messsage?s=<string>.

  
# Part 2: Bugs from lab3
  
I tested reversed(int[] arr) with the default testReverse()
 
 <pre><code>
 @Test
  public void testReverseInPlace() {
    int[] input1 = { 1, 2, 3, 4, 5 };
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[] { 5, 4, 3, 2, 1 }, input1);
  }
</code></pre>

But as shown below, it failed and gave return "arrays first differed at element [3]; expected:[2] but was:[4]"
![Junit test fail](https://imgur.com/AIPbnF8.png)

 But if you changed the input to an array that was parrallel, it would pass. For instance, below
  <pre><code>
  @Test
  public void ogtestReversed() {
    int[] input1 = { 1, 2, 3, 2, 1 };
    ogArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[] { 1, 2, 3, 2, 1 }, input1);
  }
  </code></pre>
 
 Here is a screenshot of the test passing, despite the bug not being fixed
 ![Junit false pass](https://imgur.com/IPIGPwG.png)
  
Before any changes, it looked like this:
  
 <pre><code> 
  static void reverseInPlace(int[] arr) {
        for (int i = 0; i < arr.length; i += 1) {
            arr[i] = arr[arr.length - i - 1];
        }                                
    }
   </code></pre>
         

The bug here was that it was overwriting one, without saving the value that was overwritten. This meant that at the halfway point, the first half was a reverse of the second half. However, since the first half was not stored, it copies the same value. For instance, [1,2,3,4,5] would become [5,2,3,4,5] after one iteration. At i=4, it would replace the last entry with the first entry, which should have been one. But now it is 5, so it will copy 5 instead. So the symptom was that only the second half of the original array was reversed, but the second half would not be changed. To fix this bug, I created a temp placeholder. It would store the element before it was replaced, then copy it to the opposite end. However, if the for loop is still the full array length, it will have an almost identical symptom. [1,2,3,4,5] will end up [1,2,3,2,1], meaning the first half of the array doesnt change, but the second half is reversed correctly. So to prevent this, I had the for loop stop halfway, as it would have already reversed the array at that point.
                                       
  <pre><code>                             
    static void reverseInPlace(int[] arr) {
    int temp;
    int halfway = arr.length / 2;
    for (int i = 0; i < halfway; i++) {
      temp = arr[i];
      arr[i] = arr[arr.length - i - 1];
      arr[arr.length - i - 1] = temp;
    }
 </code></pre>
  
So now, the method fufills the intended purpose of creating & returning a new array that is the reverse of the original. This is because it copies the elements of arr into newArray properly, and returns newArray as intended.
  
# Part 3: What I learned
I learned a lot this week. For one, I learned how to make a server in java and use the URI methods. I also learned how to use Junit tests which seems to be very helpful. The types of errors that I can catch using Junit tests are what usually send me to the cse basement for tutors, which usually have a long wait. So now I can solve more issues on my own, and reduce the amount of time I stare at Gradescope in frustration.
