---
layout: post
title: "LeetCode Decode Ways"
date: 2014-11-09 15:07:53 -0500
comments: true
categories: 
---

I encounter this problem in an interview with "F" company. Back then, I was not aware of anything that related to **Dynamic Programming**. After a long time of study this technique in Prof. Kingsford's course, I am gradually becoming a master in it. Enjoy it!

http://www.ninechapter.com/solutions/decode-ways/
	
In this problem, the optimal structure can be defined that *num[i] is the number of different string representations for all the number before and with ith number included*. Therefore, we have $$num[i] = \begin{cases}num[i] + num[i - 1] \\ \qquad \text{if number in postion[i - 1] match the letter in alphabet} \\ num[i] + num[i - 2] \\ \qquad \text{if number in postion[i - 2] and in postion[i - 1] form a letter, e.g. 26 means 'z'}\end{cases}$$. Moreover, it also needs a base case to main the coherence of indices since we use one variable to trace both the num array and individual character in input number string. The base case is when i equals to 0, num[0] is 1.	 
	
	/**
	*
	*/
	public class Solution {
    	public int numDecodings(String s) {
        	if (s == null || s.length() == 0) {
            	return 0;
        	}
        	int[] num = new int[s.length() + 1];
        	num[0] = 1;
        	num[1] = s.charAt(0) != '0' ? 1 : 0;
        	for (int i = 2; i <= s.length(); i++) {
            	if (s.charAt(i - 1) != '0') {
                	num[i] += num[i - 1];
            	}
            
            	int twoDigits = (s.charAt(i - 2) - '0') * 10 + (s.charAt(i - 1) - '0');
            	if ((twoDigits <= 26) && (twoDigits >= 10)) {
                	num[i] += num[i - 2];
            	}
            	// Use to trace the current status
            	// System.out.println("Current letter is" + s.charAt(i - 1));
            	// System.out.println("num" + i + "=" + num[i]);
        	}
        	return num[s.length()];
    	}
    	// Test main method
    	public static void main(String [] args) {
        	System.out.println((new Solution()).numDecodings(args[0]));
    	}
	}