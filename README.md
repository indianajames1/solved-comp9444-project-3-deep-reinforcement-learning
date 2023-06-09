Download Link: https://assignmentchef.com/product/solved-comp9444-project-3-deep-reinforcement-learning
<br>
<h3>Introduction</h3>

In this assignment we will implement a Deep Reinforcement Learning algorithm on a classic control task in the OpenAI AI-Gym Environment. Specifically, we will implement Q-Learning using a Neural Network as an approximator for the Q-function.

There are no constraints on the structure of network to be implemented; marks are awarded on the basis of the learning speed and generality of your final model.

Because we are not operating on raw pixel values but already encoded state values, the training time for this assignment is relatively short, and each training run should only require approximately 15 minutes on a standard laptop PC.




<h3>Preliminaries</h3>

We will be using the AI-Gym environment provided by OpenAI to test our algorithms. AI Gym is a toolkit that exposes a series of high-level function calls to common environment simulations used to benchmark RL algorithms. All AI Gym function calls required for this assignment have been implemented in the skeleton code provided, however it would be a good idea to understand the basic functionality by reading through the <a href="https://gym.openai.com/docs/">getting started guide</a>.

You can install AI Gym by running

<code>pip install gym</code>

or, if you need admin priveliges to install python packages:

<code>sudo -H pip install gym</code>

This will install all environments in the “toy_text”, “algorithmic”, and “classic_control” categories. We will only be using the CartPole environment in “classic_control”. If you want to only install the classic_control environments (to save disk space or to keep your installed packages at a minimum) you can run:

<code>pip install 'gym[classic_control]'</code>(prepend <code>sudo -H</code> if needed)

To test if the required environments have been installed correctly, run (in a python interpreter):

<pre>import gymenv = gym.make('CartPole-v0')env.reset()env.render()</pre>

You should then be able to see a still-frame render of the CartPole environment.




Next, download the starter code from <a href="https://www.cse.unsw.edu.au/~cs9444/18s2/hw3/src.zip"><code>src.zip</code></a>All functionality should be implemented in this file:

<blockquote>

 <code>Neural_QTrain.py</code>

</blockquote>

<h3>How to run the code</h3>

The structure of this assignment is somewhat different to Assignment 1 and 2. Instead of functions to implement in a separate file, you have been given a partially completed python script.




Once you have implemented the unfinished parts of the script, you can run the code the same way you have with <code>python3 Neural_QTrain.py</code>. There is no main loop, the script simply runs in the order it is declared.




<h3>Code Structure</h3>

The sections that are marked with <code>TODO:</code> are the ones you should complete. You are not required to structure your code in this way – if you have a solution that is more compact and achieves good results without them you are welcome to do that. Code that is marked with <code>--DO NOT MODIFY--</code> must not be modified and must be run as part of the script (i.e. don’t surround it by a conditional that is never True). This refers to the entire code-block following the comment. Lines 105 to 120, for example, must all remain unedited. Some code is marked with neither – this code is there to help you start but you may want to modify it as you increase the complexity of your solution. You may add code anywhere else you feel is appropriate.

A breif explanation of key parts of the script follows:




Placeholders:

<ul>

 <li><code>state_in</code> takes the current state of the environment, which is represented in our case as a sequence of reals.</li>

 <li><code>action_in</code> accepts a one-hot action input. It should be used to “mask” the q-values output tensor and return a q-value for that action.</li>

 <li><code>target_in</code> is the Q-value we want to move the network towards producing. Note that this target value is not fixed – this is one of the components that seperates RL from other forms of machine learning.</li>

</ul>

Network Graph:

<ul>

 <li>You can define any type of graph you like here, cnn, dense, lstm etc. It’s important to consider what is the constraint in this problem – is a larger network necessarily better?</li>

 <li><code>q_values</code>: Tensor containing Q-values for all available actions i.e. if the action space is 8 this will be a rank-1 tensor of length 8</li>

 <li><code>q_action</code>: This should be a rank-1 tensor containing 1 element. This value should be the q-value for the action set in the action_in placeholder</li>

 <li>Loss/Optimizer Definition You can define any loss function you feel is appropriate. Hint: should be a function of target_in and q_action. You should also make careful choice of the optimizer to use.</li>

</ul>

Main Loop:

<ul>

 <li>Move through the environment collecting experience. In the naive implementation, we take a step and collect the reward. We then re-calcuate the Q-value for the previous state and run an update based on this new Q-value. This is the “target” referred to throughout the code.</li>

</ul>

<h3>Implementation Steps</h3>

We will be implementing Neural Q-Learning. The details of this algorithm can be found in the Deep Reinforcement Learning lecture slides – page 12. The following tasks outline the suggested way of approaching the assignment. You are not required to implement functionality in this order, and each task is not assigned individual marks – they are a guide only.




<h4>Step 1: Basic Implementation</h4>

The first thing you should do is complete the specified TODO sections and implement a basic version of neural-q-learning that is able to perform some learning. Following the provided structure, we perform one update immediately after each step taken in the environment. This results in slow and brittle learning. A correct implementation will first achieve 200 reward in around 1000 episodes depending on hyperparameter choices. In general you will observe sporadic and unstable learning.




Details of the CartPole environment are availible <a href="https://github.com/openai/gym/wiki/CartPole-v0">here</a>.




<h4>Step 2: Batching</h4>

An easy way to speed up training is to collect batches of experiences then compute the update step on this batch. This has the effect of ensuring the Q-values are updated over a larger number of steps in the environment, however these steps will remain highly correlated. Depending on you network and hyperparameters, you should see a small to medium improvement in accuracy.

<h4>Step 3: Experience Replay</h4>

To ensure batches are decorrelated, save the experiences gathered by stepping through the environment into an array, then sample from this array at random to create the batch. This should significantly improve the robustness and stability of learning. At this point you should be able to achieve 200 average reward within the first 200 episodes.

<h4>Step 4: Extras</h4>

Once these steps have been implemented, you are free to include any extra features you feel will improve the algorithm. Use of a target network, hyperparameter tuning, and altering the Bellman update may all be benificial.

<h4>Step 5: Report</h4>

As with Assignment 2, you are required to submit a simple 1-page pdf or text file outlining and justfying your design choices. This report will be used to assign marks in accordance with the marking scheme if we are unable to run your code.

<h3>Marks:</h3>

Marks are assigned on the basis of learning speed and stability. Specifically, what is the reward at 100 episodes, and, once 200 is reached, does the performance ever degrade. Your code will be run once, so it is important that your learning rate is consistent, as well as being fast.




The provisional marking schema is:

<ul>

 <li>no run/incomplete: marks based on report, capped at 5</li>

 <li>Any learning shown over 500 episodes: 5 marks.</li>

</ul>

At ep 100, reward is:

<ul>

 <li>&gt;20: 6 marks</li>

 <li>&gt;40: 7 marks</li>

 <li>&gt;60: 8 marks</li>

 <li>&gt;80: 9 marks</li>

 <li>&gt;100: 10 marks</li>

 <li>&gt;120: 11 marks</li>

 <li>&gt;150: 12 marks</li>

 <li>&gt;170: 13 marks</li>

 <li>&gt;180: 14 marks</li>

 <li>&gt;190: 15 marks</li>

</ul>

If learning is not monotonic and stable (i.e. reward decreases by more than 10 between test runs): -2 marks

<h3>Restrictions</h3>

<ol>

 <li>The only other major restriction aside from the do not modify blocks is that for each increment of the step and episode variables in the main loop, env.step() may be called only once. In other words, you may not collect multiple state action pairs in a single iteration.</li>

 <li><b>Do not submit a file that prints extra information to std out other than what is already present. </b>Of course during development this is fine, but make sure it is removed on submission.</li>

 <li>The env must learn with no prior knowledge – i.e. you can’t hardcode in pretrained weights or initialize the network with values that have been learned previously (although you can and should experiment with different initialization schemes.</li>

 <li>You may not import any libraries other than those already included in the file, you do not need them.</li>

 <li>Your algorithm cannot take an excessively long time to train – all runs will be timed out after 2mins on a high-end PC. If your code is taking longer than 5 minutes to reach 500 episodes when run on a laptop, you need to simplify your approach.</li>

 <li>Try to adhere to the general spirit of this assignment. It is the final assignment and may appear difficult on first glance, but by developing an understanding of the problem and provided code structure first, a solution will fall into place quickly. Trying to shoe-horn in existing code and attempting to avoid the –DO NOT MODIFY– blocks is a bad path to go down.</li>

</ol>

<h3>Groups</h3>

This assignment may be done individually, or in groups of two students. In the same way as for Assignment 2, groups are determined by an SMS field called hw3group. Every student has initially been assigned a unique hw3group which is “h” followed by their student ID number, e.g. h1234567. If you plan to complete the assignment individually, you don’t need to do anything (but, if you do create a group with only you as a member, that’s ok too). If you wish to work with a partner, one of you needs to create a group in WebCMS and add the other person. Click on “Groups” in the menu to the left. Then click “Create”. Click on the menu for “Group Type” and select “hw3”. After creating a group, click “Edit”, search for the other member, and click “Add”. WebCMS assigns a unique group ID to each group, in the form of “g” followed by six digits (e.g. g 012345). We will periodically run a script to load these values into SMS.

<h3>Submission</h3>

You should submit the assignment with

<code>give cs9444 hw3 Neural_QTrain.py report.pdf</code>

You can submit as many times as you like – later submissions will overwrite earlier ones, and submissions by either group member will overwrite those of the other. You can check that your submission has been received by using the following command:

<code>9444 classrun -check</code>

The submission deadline is Sunday 21 October, 23:59.15% penalty will be applied to the (maximum) mark for every 24 hours late after the deadline.

Questions relating to the project can be posted to the Forums on the course Web page.

Additional information may be found in the <a href="https://www.cse.unsw.edu.au/~cs9444/18s2/hw3/faq.shtml">FAQ</a> and will be considered as part of the specification for the project. You should check this page regularly.

If you believe you are getting an error due to a bug in the specification or provided code, you can post to the forums on the course Web page. Please only post queries after you have made a reasonable effort to resolve the problem yourself. If you have a generic request for help you should attend one of the lab consultation sessions during the week.

You should always adhere to good coding practices and style. In general, a program that attempts a substantial part of the job but does that part correctly will receive more marks than one attempting to do the entire job but with many errors.


