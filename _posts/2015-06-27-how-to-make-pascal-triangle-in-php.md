---
layout: post
title: "How to Make Pascal Triangle in PHP"
date: 2012-06-27 16:52:00 +0700
categories: php
meta_keywords: "pascal triangle, php"
meta_description: "Step by step create a pascal triangle in PHP"
comments: true
---

A few days ago, my friend asked me how to make pascal triangle in PHP. Then, I start coding to make it. And now, I want to share my code with you. Hope this post will help you to solve your problem in pascal triangle with PHP.

Before start, I will inform you that this tutorial will divided to be 4 parts, thats:

1. Make the number to show in an array and show it also in array,
2. Show the number with table in left alignment,
3. Show the number with table in right alignment,
4. And, show the number with table in center alignment

Our goal is make pascal triangle like this picture below

![Pascal Triangle PHP](/assets/images/posts/pascal-step4.jpg "Pascal Triangle PHP Goal")

Let’s start our coding!

{% highlight php %}
<html>
<head>
  <title>Pascal Triangle in PHP</title>
</head>
<body>
<?php $level = $_POST['line']; ?>
<div>
  <form action="" method="post">
  Input Line Number <input type="text" name="line" value="<?php echo $level; ?>"> <input type="submit" value="Submit">
  </form>
</div>
<div>
<table>
<?php		
/* step 1 */	
for ($y = 1; $y <= $level; $y ++){
  for ($x = 1; $x <= $y; $x ++){
    if($x == 1){
      $number[$y][$x] = 1; // start with 1
    }elseif($x == $y){
      $number[$y][$x] = 1; // end with 1
    }else{
      $number[$y][$x] = $number[$y-1][$x-1] + $number[$y-1][$x];
    }			
  }
}

//to show the result in array
//we will delete this code in step 2
echo "<pre>";
print_r($number);
echo "</'pre>";
?>
</table>
</div>
</body>
</html>
{% endhighlight %}

Save the file in your localhost folder. I will explain some codes.

- First, we make a input field, so we can try it dynamicly.
- Second, we process the form and do 2 looping.
$y as vertical looping/deep level that user inputed from the form and $x as horizontal looping.

Now, try to access the file from browser with localhost. This is the screenshot from my browser. I try with 5 level.

![Pascal Triangle PHP Step 1](/assets/images/posts/pascal-step1.jpg "Pascal Triangle PHP Step 1")

Okay, step 1 has finished. Now, we will try to show the number in table, but still in left alignment. This is the code.

{% highlight php %}
<html>
<head>
  <title>Pascal Triangle in PHP</title>
</head>
<body>
<?php $level = $_POST['line']; ?>
<div>
  <form action="" method="post">
  Input Line Number <input type="text" name="line" value="<?php echo $level; ?>"> <input type="submit" value="Submit">
  </form>
</div>
<div>
<table>
<?php
/* step 2 */
for ($y = 1; $y <= $level; $y ++){
  echo "<tr>";
  for ($x = 1; $x <= $y; $x ++){
    if($x == 1){
      $number[$y][$x] = 1; // start with 1
      echo "<td>".$number[$y][$x]."</td>";
    }elseif($x == $y){
      $number[$y][$x] = 1; // end with 1
      echo "<td>".$number[$y][$x]."</td>";
    }else{
      $number[$y][$x] = $number[$y-1][$x-1] + $number[$y-1][$x];
      echo "<td>".$number[$y][$x]."</td>";
    }
  }
  echo "</tr>";
}
?>
</table>
</div>
</body>
</html>
{% endhighlight %}

Save it on your localhost folder. And this file will print the result in browser like this picture:

![Pascal Triangle PHP Step 2](/assets/images/posts/pascal-step2.jpg "Pascal Triangle PHP Step 2")

In this file, we delete the print_r() function, because we change it with echo in the looping. Now, please refresh your browser and it will show better than in step 1. Okay, let’s continue to step 3, in this step we will show the data with tab in the left, so I will show like in right alignment. Below is the code.

{% highlight php %}
<html>
<head>
  <title>Pascal Triangle in PHP</title>
</head>
<body>
<?php $level = $_POST['line']; ?>
<div>
  <form action="" method="post">
  Input Line Number <input type="text" name="line" value="<?php echo $level; ?>"> <input type="submit" value="Submit">
  </form>
</div>
<div>
<table>
<?php
/* step 3 */
for ($y = 1; $y <= $level; $y ++){
  echo "<tr>";
  for ($x = 1; $x <= $y; $x ++){
    if($x == 1){
      $number[$y][$x] = 1; // start with 1
      
      //show tab from left
      if($level != $y){
        echo "<td colspan='".($level-$y)."'></td>";
      }
      
      echo "<td>".$number[$y][$x]."</td>";
    }elseif($x == $y){
      $number[$y][$x] = 1; // end with 1
      echo "<td>".$number[$y][$x]."</td>";
    }else{
      $number[$y][$x] = $number[$y-1][$x-1] + $number[$y-1][$x];
      echo "<td>".$number[$y][$x]."</td>";
    }
  }
  echo "</tr>";
}
?>
</table>
</div>
</body>
</html>
{% endhighlight %}

Save it! And you will see like this picture when you refresh your browser.

![Pascal Triangle PHP Step 3](/assets/images/posts/pascal-step3.jpg "Pascal Triangle PHP Step 3")

This tutorial almost finish, in the last step we will make the result in center aligment like our goal. And we need write two line codes only. This is the finish code. Let’s type it!

{% highlight php %}
<html>
<head>
  <title>Pascal Triangle in PHP</title>
</head>
<body>
<?php $level = $_POST['line']; ?>
<div>
  <form action="" method="post">
  Input Line Number <input type="text" name="line" value="<?php echo $level; ?>"> <input type="submit" value="Submit">
  </form>
</div>
<div>
<table>
<?php
/* step 4 - finish */	
for ($y = 1; $y <= $level; $y ++){
  echo "<tr>";
  for ($x = 1; $x <= $y; $x ++){
    if($x == 1){
      $number[$y][$x] = 1; // start with 1
      
      //show tab from left
      if($level != $y){
        echo "<td colspan='".($level-$y)."'></td>";
      }
      
      echo "<td>".$number[$y][$x]."</td>";
      echo "<td></td>"; //show tab
    }elseif($x == $y){
      $number[$y][$x] = 1; // end with 1
      echo "<td>".$number[$y][$x]."</td>";
    }else{
      $number[$y][$x] = $number[$y-1][$x-1] + $number[$y-1][$x];
      echo "<td>".$number[$y][$x]."</td>";
      echo "<td></td>"; //show tab
    }
  }
  echo "</tr>";
}
?>
</table>
</div>
</body>
</html>
{% endhighlight %}

Save and refresh your browser. If you write the same code, it will print like this.

![Pascal Triangle PHP Step 4](/assets/images/posts/pascal-step4.jpg "Pascal Triangle PHP Step 4")

Finish, thank you to read my post. Please give your comment.