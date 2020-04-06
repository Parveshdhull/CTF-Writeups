Thanks, Midnight Sun CTF team for this amazing CTF and also for providing payload for this challenge without that this writeup could not have been possible.

Code: http://crossintheroof-01.play.midnightsunctf.se:3000/?source

<?php
 header('X-XSS-Protection: 0');
 header('X-Frame-Options: deny');
 header('X-Content-Type-Options: nosniff');
 header('Content-Type: text/html; charset=UTF-8');

if(!isset($_GET['xss'])){
    if(isset($_GET['error'])){
        die('stop haking me!!!');
    }

    if(isset($_GET['source'])){
        highlight_file('index.php');
        die();
    }

    die('unluky');
}

 $xss = $_GET['xss']?$_GET['xss']:"";
 $xss = preg_replace("|[}/{]|", "", $xss);

?>

<script>
setTimeout(function(){
    try{
        return location = '/?i_said_no_xss_4_u_:)';
        nodice=<?php echo $xss; ?>;
    }catch(err){
        return location = '/?error='+<?php echo $xss; ?>;
    }
    },500);
</script>
<script>
/* 
    payload: <?php echo $xss ?>

*/
</script>
<body onload='location="/?no_xss_4_u_:)"'>hi. bye.</body>

Explanation:
Part 1: In the above code 1st part is PHP code which checks what parameter is passed and according to that it loads the page. For Ex. this source code page is accessed by passing the source parameter. And it also removes harmful characters. And when parameter XSS is passed it skips page loading portion, So this part of code has no use to us because we are going to use the XSS parameter.

Part 2: 
In the above code, payload XSS is reflected at three places, try block, catch block and comment section.
Let ?xss=alert(1)
Then Try block will not even execute this code due to return call before it, and catch block will not be called because no error happens and comment is just comment, So no XSS.

Payload: 

    http://crossintheroof-01.play.midnightsunctf.se:3000/?xss=alert(1);let%20location=1;%0a%3C!--%3Cscript

    Decoded:
    http://crossintheroof-01.play.midnightsunctf.se:3000/?xss=alert(1);let location=1;
    <!--<script

So what this will do to each reflection?
Try-Block:

In JavaScript no matter where functions and variables are declared, they are moved to the top of their scope regardless of whether their scope is global or local. This is called JavaScript hoisting.
For More Details Read:
https://www.geeksforgeeks.org/javascript-hoisting/
Due to this in try block firefox will give the error "Can't access lexical declaration 'location' before initialization" and Chrome will give the error "Cannot access 'location' before initialization"
For More info about error read:
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Errors/Cant_access_lexical_declaration_before_init

So, Try-Block is over and it will create error and that will call catch-Block

Catch-Block:
The catch block will execute alert(1). So our task of the challenge is complete?
No, It will still not show the alert.
Why?
Because in our code our try-block is delayed by 500ms, and before this will be complete last line of code(body section) will execute and will load new page "/?no_xss_4_u_"
So we have to stop this part of code to executed and here comes part-3

Part 3: 
Comment Section:
In payload part "<!--<script" will convert whole upcoming code,(including body) into comment.
Why?
I was also confused by this behavior, So I asked it on StackExchange and one good person was able to explain this behavior.
Reason: https://security.stackexchange.com/questions/229289/effect-of-script-tag-inside-html-comment

Final Words:
So we completed this challenge in three steps. First, we created an error in try-block and then we executed payload in catch-block, And after that, with the help of the comment section, we stopped loading of the new page.
I am not an expert and this is my first writeup. I don't know if this writeup is up to mark, But I tried my best to give accurate information.
