## Getting started 
#### Entry: <span id="index"></span>, Published: <span id="published"></span>

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>


This week Is the start of study block 2 at university and we begin our machine learning module (COMP704). In our first workshop, we are getting to grips with some of the libraries available in python3, such as Open Ai Gym[[2](#c2)] and scikit-learn[[1](#c1)] among others. It should have been relatively straight forwards, however some of libraries wouldn’t install via Python-pip or they wouldn’t include at run-time. What was going on.

It all comes down to the finer details. The computers at university are not configure the same way I have my python3 configured at home, which lead to on oversight when setting up a new virtual environment in PyCharm. I gave it a save location, name and selected the default Python3.7 interpreter and none of the AI packages would install via python-pip. I tried a number of things, however, it was fruitless. Evenly we released that scikit-learn required the 64-bit version of python. Quickly opening the default python interpreter, did indeed show that it was the 32-bit version. So, there is our issue. Thankfully, when selecting a different interpreter in PyCharm it revealed that the 64-bit version was install too, which made a real easy solution for the problem.

To be honest this could have been avoided if I had just read the documentation correctly on the scikit-learn website. Furthermore, I should have really checked what versions of python were installed as I would normally would select the 64-bit version. Why was the 32-bit version even installed, it seems kind of pointless on a 64-bit machine but or well. We’ve leant that we need to pay more attention to the finer details.


<br />

### Cites
##### All citations are available in a single [bibtex file](../references.bib)

<p id="c1">
[1]  F. Pedregosa, G. Varoquaux, A. Gramfort, V. Michel, B. Thirion, O. Grisel, M. Blondel, P. Prettenhofer, R. Weiss, V. Dubourg, J. Vanderplas, A. Passos, D. Cournapeau, M. Brucher, M. Perrot, and E. Duchesnay, “Scikit-learn: Machine learning in Python,” Journal of Machine Learning Research, vol. 12, pp. 2825–2830, 2011.
</p>
<p id="c2">
[2] G. Brockman, V. Cheung, L. Pettersson, J. Schneider, J. Schulman, J. Tang, and W. Zaremba, “Openai gym,” arXiv preprint arXiv:1606.01540, 2016.

List of Figures
1

</p>

<br />
<br />

<span class="priv_entry" style="display: inline;"></span>
| 
[Return to index](../)
| 
<span class="next_entry" style="display: inline;"></span>

<br />
<br />

**Please refer to the [Licences and Sources](../resources/licences-and-sources) document for content used from external sources along with usage and licence infomation**

<br />

<script>
// Store the entry id and published values in a JS script, to make life easier with updateing links.
entry_id  = 0
published = "27-01-22" 
week = 1

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