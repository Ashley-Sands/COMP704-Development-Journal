## [Tital Goes Here] - Journal Entry Template 
#### Entry: <span id="index"></span>, Published: <span id="published"></span>

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>


# Something’s not right...

After a bit more training (350k-500k timesteps) it started to become apparent that something was not quite right. It either gets really close to collecting the goals or it avoids them at the last moment [Fig. 10]. 

[Fig 10.1. Avoids collectables at the last minute]
OR
[Fig 10.2. Just flies through the middle two cells]

At first I had a little play with the reward values, decreasing the ``surviving`` from ``1`` to ``0`` and increasing the goal value from ``20`` to ``60``, might make the goal more attractive to the agent however it never really made that much difference, no matter what I changed the values to. So, I decided to give a different model a try namely ``Proximal Policy Optimization`` (PPO) (which is based on A2C and TRPO). All-in-all ``PPO`` seems to be more stable along with surviving for a much longer time. However, it seems to of figured out that it can just fly in the middle two cells and never collide [Fig. 11]. This is possible since the level generator never spawns colliders in the middle two cells, maybe I should do something about that. 

[Fig 11. Flies through the middle 2 cells]

So, I decided to stick with PPO since it seems more stable and survives longer. Now I thought it would be a good time to start tweaking the ``learning rate`` and ``gamma`` values. At first, I went for a more extreme ``learning rate`` of ``0.1`` (Default is: ``0.0003``) and it just learn to die after ``100,000`` training timesteps. Following that I went for something more reasonable and tried ``0.0005`` and ``0.0001``.

(It might be worth noting that I also fixed an issue that was sometimes causing the collectables to spawn in invalid locations.)

[Fig. 11.1, LR 0.0005]
[Fig. 11.2, LR 0.0001]

Either way, increasing/decreasing the learning rate made next to no difference to the outcome of the agent. So, I moved onto the ``gamma`` value with is ``0.99`` by default. Basically a lower gamma fevers the immediate reward while a higher gamma looks for the reward further ahead. I drop the gamma to ``0.5`` witch I thought would be quite an extreme change, however it didn’t make that much difference at all [Fig 12.2]. So, the next logical step would be to drop the gamma to 0, just to see if there’s a more noticeable change. 

[Fig. 12.1 Gamma 0.5]
[Fig. 12.2 Gamma 0]

Along with changing the ``gamma`` and ``Learning rate`` I further tried different rewards however, none of that seemed to work, either. Next I started to wonder if changing the surviving rewards from a fixed value to a dynamic value based on the distance to the goal. This way the agent will receive a greater reward for getting closer to the goal and therefor hopefully make it more attractive. For the training I put the default value back for the ``learning rate`` and ``gamma`` values while reducing the batch size to ``32`` (default is ``64``). That too makes absolutely no difference [Fig 13] and it still just favers the middle two cells, if anything its favouring them more than in previous generations.

[Fig 13. dynamic scoring]

Since it always seems to faver the middle two cells, I gathered the next step would be to change the level generation to prevent a straight-line path through the centre. At least it would force the agent to learn to explore outside of the middle two cells, and hopefully start going for the goals [Fig.13]. 

[Fig 13. New level generation]

While it does seem a little bit better it’s still not right, and avoid the goals below it. So, my next thought was to change the observation space. Rather than supplying collider cells as on or off, maybe it would be better to supply the distance to the collision for above and below the helicopter, for up 3 cells in front [Fig 14].

[Fig 14, displays distance sensors for up to 3 in front]

I quite like this idea as it would mean that I can replace all the avoid keys in the ``observation space`` to a single key, with a value of ``Box`` space which contains all the distance values. Since I am only supplying the distance for our current cell to three cells in front, we will need eight values in the ``Box`` space (four above and four below). This means that the ``observation space`` is now defined as

```python
observation_space = {
  "Player position": Box( [y_min], [y_max], dtype=np.float )
  "speed": Box( [min_y_speed], [max_y_speed], dtype=np.float ),
  "Goal Position": Box( [x_min, y_min], [x_max, y_max], dtype=np.float ), 
  "Obstacles": Box( [x_min_0, x_min_1, ... x_min_7], [x_max_0, x_max_1, ... x_max_7], dtype=np.float )
}
```
It seems to of made quite a difference to the agent behaviour when it comes to navigating the environment, however it still doesn’t particularly go out of it way to collect the goals [Fig 15]. At this point iv came to a conclusion that there must be something wrong with the observation space that I just can’t finger out with my current knowledge. So, I decided to move onto another game snake, with the hope that it will give me some new ideas to resolve the issues in the helicopter game.

[Fig 15. Agent with the new observation space defined]


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
entry_id  = 5
published = "18-02-22" 
week = 4

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