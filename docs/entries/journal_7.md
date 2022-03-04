## Genetic Algorithm
#### Entry: <span id="index"></span>, Published: <span id="published"></span>

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>

Over the past week or so, I’ve been working on implementing a system for the genetic algorithm (GA). For this we were given a very basic outline demonstrating the necessary function for the genetic algorithm such as, the initial population generation, population selection and crossover method along with basic catching. While the demonstration was of great help the structure of the application, it wasn’t particularly durable. It just pretty much just implemented the necessary methods.

While I was going through the tutorials, I noticed that there was a lot of common functionality with the previous demonstrations. Therefore, I decided I should build on top of my previous work (witch left me with a bit of technical debt in the end, not going lie) to make something much more flexible and durable. So, I went about implementing two new components (classes), 1. A ``PopulationHandler`` and 2. A ``GAAGent``. The idea was to only implement the necessary functionality for the genetic algorithm, while creating an instance of the previous components, which are set in to the AG objects.

1. ``Population Handler`` is responsible for generating the initial population, selecting the next population using the tournament method (or a method defined by user) and orchestrating the training or each agent.

2. ``GAAgent`` handles the ML model including loading and unloading the model, training the agent itself, catching the fitness, crossing over the genomes of two parent agents along with generating new genomes. Furthermore, it has helper methods for predicting inputs and playing back (run) the trained model.

### The issue (threading)
One thing I found with the genetic algorithm implementation is it can take a substantial amount of time of train each agent in the population individually. Therefor I wanted to run multiple environments in parallel. So, I attempted to train multiple agents in parallel using ``CUDA``, however it would either deadlock or take longer with unusual results. At first I put this down to the amount of time it takes to upload the model to the GPU and maybe something going on in pyTorch. So, I refactor a large amount of code to allow me to train 1 model on ``CUDA`` and another model on ``CPU`` and the same thing happened. After a bit of digging around I noticed that only a single PyGame instance was actually lunching, which I now think is the root cause of the issue. On a reflection I should have implement the PyGame class in a way that would allow it to run headless for training purposes. Furthermore, there could also be performance improvements if I had implemented it this way. Unfortunately I didn’t have enough time to refactor the code to make it a headless implementation.

### The debt
Throughout the project I tried to reuse as many components that I have implemented as possible to save a bit time. However, near the end of the project it started to become difficult to make changes to the system without breaking the previous components. The main component that suffered from this was the ``Model`` class which is a wrapper for the stable baseline Model witch was reused in the ``GAAgent`` class, witch essentially made the GAAgent a ``genetic algorithm`` wrapper for the ``Model`` wrapper. 

Looking back, I should have either done one of two things. I either should of just implemented it into the ``GAAgent`` or I should have made the ``GAAgent`` inherit from the ``Model`` class which would of been my preferred method out of the two. If I where to continue this project that would be in the next stage of development so I dont have to continue making the ``Model`` class backwards compatible.




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
entry_id  = 7
published = "02-03-22" 
week = -1

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