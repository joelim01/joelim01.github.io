---
layout: post
title:  "From the edge of the world to character islands pt1"
date:   2017-01-21 23:27:43 +0000
---


Well, that's it. I'm done. I've officially graduated from Flatirons Web Development Online program. I'm past all the hard stuff. Time to just dust off my hands, sit back, and wait for those developer jobs to *roll* on it.  

Doesn't quite sound right to you? Yeah, me either.

Now, perhaps more than ever, is a time when I need to be disciplined and focused on learning. While Flatiron's program was wonderful at giving me the foundation and practical knowledge I would need to build my confidence to seek a job in code, there are a whole set of skills that developers can and will be judged by in their technical interviews that I need to brush up on. 

I've been working through lots, from JS scopes and inheritance to data structures (binary trees are cool!) and reading and trying to absorb as much as possible.

That's between meetups, reaching out to people in the tech community, coding my new personal site www.sayhellojoe.com and work. Its exciting and a little bit daunting.

I want to talk a little bit about a code challenge from www.hackerrank.com that I am working on. I didn't immediately see that it is in the 'expert' category. I know you might be scoffing right now, but when I tell you that out of the 686 people who have tried it only 146 have nailed it, which is a far cry from the 85-95% completion for medium and easy questions, you should believe me that it is no picnic.

I used to ask myself how you could objectively measure if one coding solution was better than another. I could think about readability, comprehensibility, ease of deployment (which all seem pretty subjective to me) but after getting some exposure to big O notation and having to work through some problems on hackerrank that would fail if the solution wasn't efficient enough, I have a new appreciation for the objectivity of resource intensiveness. 

Let's look at this problem together. [Here's a link](https://www.hackerrank.com/challenges/letter-islands).

The cliff notes are that you will be given a string of lowercase characters and an integer *frequency*. It is your job to find all the substrings that appear in the string and are not bounded by another instance of the substring *frequency* number of times.

**Sample Input**
abaab
2

**Sample Output**
3

**Explanation**
All the suitable substrings are: a, ab, b.

Ok, let's break this down. I'll want to find all of the unique substrings in the given string.

Here is a super readable code snippet:

```
s = ""
i = 0
a = 0

s = gets.strip
i = gets.to_i

siz = s.length
answer = []

#for each index of the string
(0..siz-1).each do |n|
    # march from the left to the right
    (n..siz-1).each do |i|
        # and iterate to insert all the sub-strings from n to the end of the full string into the answers array
        # ie string = abcdefg, n = 3, insert defg, efg, fg
        answer << s[n..i]
    end
end
#return unique values
answer.uniq
```

I could do the same thing in a much more terse style (which I do not prefer for readability's sake):
```
# put all the indicies of the string into an array
# make an array of all possible combinations of elements
# ie [1,2,3].product([4,5])     #=> [[1,4],[1,5],[2,4],[2,5],[3,4],[3,5]]
# reject any combinations where the first number is greater than the second number (can't end before we begin!)
# we are going to use these start and end index numbers to generate our substrings
# collect all the unique strings using the array of substring indicies generated in the previous step 

indices = (0...s.length).to_a
s_array = indices.product(indices).reject{|i,j| i > j}.map{|i,j| s[i..j]}.uniq

```

Then I'll need to take those substrings and march through the given string to locate all of the islands.

But... really none of that matters in the face of this particular challenge, which requires suffix trees to solve within the time constraints (which only I managed to glean once I produced a working but too slow solution and read the comments of other coders seeking to solve this difficult challenge).

So this will have to serve as part 1 in my odyssey.

Next stop? [Suffix trees](http://stanford.edu/~mjkay/suffix_tree.pdf)!

