## Genetic Algorithm
#### Entry: <span id="index"></span>, Published: <span id="published"></span>

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>

Over the past week or so, iv been working on implerement a system for the genetic algorithm (GA). For this we where given a very basic outline demonstratining the nessesary function for the genetic algorithm such as, the initial population generation, population selection and crossover method along with basic catching. While the demonstration was of great help the structure of the appliction wasent particly durable. It just prity much just implumented the nessery methods.

While i was going through the tutoralials, i noticesed that there was a lot of common functionality with the privioues demonstrations. Therefor, i decided i should build on top of my privious work (witch left me with a bit of tecnical debt in the end, not going lie) to make somthink muct more felexable and durable. So i went about implermenting two new components (classes), 1. A ``PopulationHandler`` and 2. A ``GAAGent``. The idea was to only implement the nessesary functionality for the genetic algorithm, while creating an instance of the privious components, witch are set in to the AG objects.

1. ``Population Handler``
2. ``GAAgent``

... A bit more about the stucture

### The issue (threading)

### The debt

### What i should of done


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