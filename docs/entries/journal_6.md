## [Tital Goes Here] - Journal Entry Template 
#### Entry: <span id="index"></span>, Published: <span id="published"></span>

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>

# Snake (13/02)
After the helicopter game I wanted to try out a different environment, so I decided to make snake since it relatively easy to implerment. Furthermore, it only has a few available actions (none, up, left, down, right) for the ML agent to choose from along with a confined observation space. Since the agent can only choose one of five actions at any one time, meant I could define the action space as ``Discrete(5)`` witch made it simple to implement. For snake defining the observation space, was a bit easier than the helicopter game (probably because it was my second attempt). Since snake is basically a 2D grid it meant I could implerment the observation space as a 1D array (``y*x_size+x = cell id``) with one of four possible values in the range of zero and three (``0=empty, 1=goal, 2=avoid, 3=snake head``). I gathered that it would be pointless to implerment a fifth for the snake tail since it something that we want to avoid, so it could fall under the same category as boundaries. Therefor both bounds and the snake tail are both defined in the observation space as ``2`` (``Avoid``). So the action and observations space has been defined in snake as

```python
action_space = Discrete( 5 )
observation_space = Box( low=0, high=3, shape=( grid_size.x * grid_size.y, ), dtype=int )
```

Now I had configured the action and observation space, I could think about setting the rewards and training parameters. For the rewards I simple set ``0`` for surviving the tick, ``1`` for collecting a goal and ``-1`` for death. While for the training parameters I set the ``learning rete`` to ``0.0003`` and the gamma to ``0.99`` while leaving the rest as default for both DQN and PPO. To begin I just wanted to see if it would work so I start with just 10,000 learning timesteps and it all seemed to work fine except it learn nothing (well it learnt to go back and forth). So I increased the learning timesteps 10 fold to 100,000 and it made absolutely no difference with either of DQN or PPO models [Fig 16].

[Fig 16. First snake agent run learn nothing after 100,000 training steps]

Following this I started systematically reducing the gamma until I reached ``0`` and increasing the timesteps up to ``600,000``, while also tinkering around with different reward values, which too made no difference to the outcome. At most it managed to learn to move to random locations and get tuck moving back and forth for the entire run. Once I noticed that the snake got stuck in different location each run, due the goal being in a different location. This at least showed that something was happened, so I began wondering if moving the goal after ``n`` amount ticks, would help the training processes and that’s exactly what I did next. I implemented a taggable training mode, to change the goal location if the agent failed to collect the goal after ``n`` ticks. Furthermore, I implemented an additional reward of ``-2`` for falling to collect the goal and further decreased the death reward to ``-10``. Since I was unsure what the gamma should be I just set to a mid-range value of ``0.5`` and left the learning rate at ``0.0001`` along with the timestamps at ``100,000``. It seemed to of had the desired effect and started to collect the goals, not many but it was an improvement [fig 17.1, 17.2].

[fig 17.1, 17.2]

Following this I once again tinkered with the gamma and learning rate and found that a gamma of ``0.5`` was actually around the sweet spot. Higher values would encourage it to take longer paths or more likely to get stuck in a loop while lower values would make it more likely to eat it tail, however a gamma value seemed better than a higher one. While on the other hand, I found that a value around ``0.0003`` for the learning rate was best. lower value around ``0.0001`` would survive longer while higher value round ``0.0005`` would collect more but die sooner.

The next stage was to implerment a method to load in the save models and put the loaded models back in to training mode (you could already load a model back in to preview it). Since if I wanted to train the model for let say ``500,00`` timesteps I would have to start again and quite frankly waste time, whereas I could just continue from ``100k``and train for an additional ``400,000`` steps. At first I was under the impression that calling the ``train`` method on the loaded model would put it back into training mode, however this was not the case and I was meet with a whole host of exceptions. After a bit of digging around the documentation and StackOverflow, I realised that there was actually a parameter on the ``learn`` method named ``reset_num_timesteps``, which does exactly was it say, resets the number of timesteps back to 0 allowing the model to train once more. So, I got that implemented into my model warper class and went about further training to see if the model would continue to improve.

That it did, after an additional ``500,000`` training timesteps there was quite a large improvement [fig 18]. Next I wanted to start training the agent for multiple session with, for example 100,000 timesteps in each. This way I would be able to evaluate how much the agent is improving after each session off 100,000 steps. Furthermore, I thought that it might be worth seeing what would happen if I changed the ``learning rate`` and ``gamma`` values systematically, for each session. I started out with an extremely high ``learning rate`` and low gamma, increasing the gamma by ``0.1`` and decreasing the ``learning rate`` by ``(0.003-0.0001)*0.1`` in each session. After six sessions of 100,000 timesteps I found that it was significantly worse then when I just did ``600,000`` timesteps, 0.5 gamma and 0.0001 learning rate [fig 19]. after playing with the learning rate and gamma a bit my finding where similar to the previous, a gamma value of around ``0.3`` and ``0.5`` seemed to work best, while a learning rate around ``0.0004`` and ``0.0002`` work best [fig 20]. 

[fig 18, retraining for an additional 500,000 steps]
[fig 19, it got worse]
[fig 20, shows the training history for 40 sessions of 250,000 training steps up to 10,000,000 steps]



<br />
<br />

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>

<br />
<br />

<script>
// Store the entry id and published values in a JS script, to make life easier with updateing links.
entry_id  = 6
published = "21-02-22" 
week = 5

document.getElementById("index").innerHTML = entry_id
document.getElementById("published").innerHTML   = `${published} (Week: ${week})`


next_page = "journal_"+ (entry_id + 1)
priv_page = "journal_"+ (entry_id - 1)

next_links = document.getElementsByClassName("next_entry")
priv_links = document.getElementsByClassName("priv_entry")

// atempt to fetch the next page. 
// if we get an ok responce display the next links, 
// otherwise we have most likely reaced the end.
fetch('./'+next_page+'.html')
    .then (
        responce => {
        if ( responce.ok ) 
            for ( let i in next_links )
                next_links[i].innerHTML = '<a href="./'+next_page+'">Next ></a>'
        }
    )

// only display the priv page link if we have gone past the first page.
// theres no need to fetch the prv page, since we know the min id is 0
if (entry_id > 0)
    for ( let i in priv_links )
        priv_links[i].innerHTML = '<a href="./'+priv_page+'">< Priv</a>'


</script>