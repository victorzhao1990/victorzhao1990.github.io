---
layout: post
title: "LeetCode Max Points on a Line"
date: 2014-11-06 11:21:41 -0500
comments: true
categories: 
---
https://oj.leetcode.com/discuss/9929/a-java-solution-with-notes

	import java.util.HashMap;
	import java.lang.Math;

	// Definition for a point.
	class Point {
    	int x;
    	int y;
    	Point() { x = 0; y = 0; }
    	Point(int a, int b) { x = a; y = b; }
	}
	/*  
	* 
 	*  O(n^2) time.
 	*
 	*  A line is determined by two factors,say y=ax+b
 	*
 	*  If two points(x1,y1) (x2,y2) are on the same line(Of course).
 	*  Consider the gap between two points.
 	*  We have (y2-y1)=a(x2-x1),a=(y2-y1)/(x2-x1) a is a rational, b is canceled since b is a constant
 	*  If a third point (x3,y3) are on the same line. So we must have y3=ax3+b
 	*  Thus,(y3-y1)/(x3-x1)=(y2-y1)/(x2-x1)=a
 	*  Since a is a rational, there exists y0 and x0, y0/x0=(y3-y1)/(x3-x1)=(y2-y1)/(x2-x1)=a
 	*  So we can use y0&x0 to track a line;
 	*/
	public class Solution {
    	// Point[] points;
    	public int maxPoints(Point[] points) {
        	if (points == null) {
            	return 0;
        	}
        
        	// If there are only two points or less, just return the number of these points.
        	if (points.length <= 2) {
            	return points.length;
        	}
        	// The value that is used to return for the final number of points
        	int result = 0;
        	// Store the number points in a variable
        	int n = points.length;
        	// Use a nested HashMap to store the information for x0, y0 and the number of points that reside on this line.
        	// The format is HashMap<x0, HashMap<y0, number>>
        	HashMap<Integer, HashMap<Integer, Integer>> map = new HashMap<>();
        	for (int i = 0; i < n; i++) {
            	// If there are several points that have the same coordinates, use a counter called overlap to record it.
            	int overlap = 0;
            	// Since it use only one nested hashmap to store the information of the point starting from one specific i index pairing with 
            	// all other points starting from i + 1 to n, the map need to be refresh during the update of i. 
            	map.clear();
            	// Use variable max to record the maximum value in every iteration/lookup in the hashmap
            	int max = 0;
            	for (int j = i + 1; j < n; j++) {
                	int x = points[i].x - points[j].x;
                	int y = points[i].y - points[j].y;
                	if ((x == 0) && (y == 0)) {
                   		overlap++;
                    	continue;
                	}
                	// Use gcd to normalize the slope for each point
                	int gcd = generateGCD(x, y);
                	if (gcd != 0) {
                    	x /= gcd;
                    	y /= gcd;
                	}
                	// If map doesn't include one specific point, add it.
                	if (map.containsKey(x)) {
                   		if (map.get(x).containsKey(y)) {
                        	map.get(x).put(y, map.get(x).get(y) + 1);
                    	} else {
                       		map.get(x).put(y, 1);
                    	}
                	} else {
                    	HashMap<Integer, Integer> innerMap = new HashMap<>();
                    	innerMap.put(y, 1);
                    	map.put(x, innerMap);
                	}
                	max = Math.max(max, map.get(x).get(y));
            	}
            	// The number of overlap should be included.
            	result = Math.max(result, max + overlap + 1);
        	}
        	return result;
    	}

    	private int generateGCD(int a, int b) {
        	if (b == 0) {
            	return a;
        	} else {
            	return generateGCD(b, a % b);
        	}
    	}
	}
