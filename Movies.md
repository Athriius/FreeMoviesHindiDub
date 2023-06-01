## Add a Movie
> Here you can add the name of a movie and any commentary about it that you have

<body>
<style>
custom-field input {
  border: 2px solid darkgrey;
  -webkit-appearance: none;
  -ms-appearance: none;
  -moz-appearance: none;
  appearance: none;
  background: #f2f2f2;
  padding: 12px;
  border-radius: 10px;
  width: 250px;
  font-size: 14px;
}
</style>
<style>
.center {
  margin: auto;
  width: 60%;
  border: 3px solid  #FFD133;
  padding: 10px;
}
.sortTitle {
  margin: auto;
  color: white;
  border: 3px solid  #FFC133;
  padding: 12px;
  margin-top: 30px;
  margin-bottom: 30px;
}
.movieBody {
  margin: auto;
  color: white;
  border: 3px solid  #FFC133;
  padding: 12px;
  width: 1000px;
  background: #f2f2f2;
}
img {
  width: 30px;
  height: 30px;
}
</style>
<form>
    <custom-field class="formBox">
        <label for="ftitle">Movie</label>
        <input type="text" id="ftitle" placeholder="Movie Title"/>
    </custom-field>
    <p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
    <custom-field class="formBox">
        <label for="commentary">Commentary</label>
        <input type="text" id="commentary" placeholder="Commentary"/>
    </custom-field>
    <p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</p>
    <custom-field class="formBox">
        <button id="btn">Click to Add</button>
    </custom-field>
    <button onclick="logSort()">Sort Movies By Title</button>
    <button onclick="hideMovies()">Hide Movies</button>
</form>
<script>
    let movies = [];
    // example {id:1592304983049, title: 'Avengers: Endgame', commentary: 'good action scenes.'}
    const addMovie = (ev)=>{
        ev.preventDefault();  //stops the form submitting automatically
        create_movie()
        let movie = {
            DateID: Date.now(),
            ftitle: document.getElementById('ftitle').value,
            commentary: document.getElementById('commentary').value,
            likes: 0
        }
        movies.push(movie);
        document.forms[0].reset(); // to clear the form for the next entries
        console.warn('added' , {movies} ); // displays array in the console
        //saving to localStorage
        localStorage.setItem('MyMovieList', JSON.stringify(movies) );
        Addmovie()
    }
    document.addEventListener('DOMContentLoaded', ()=>{
        document.getElementById('btn').addEventListener('click', addMovie);
    });
    //Title
    const newTitle = document.createElement("H1");
    newTitle.innerText = '\xa0\xa0' + "Displayed below are your movies and commentary"
    document.body.appendChild(newTitle)
    // Creating Body
    var bodyDiv = document.createElement("div");
    document.body.appendChild(bodyDiv);
    bodyDiv.classList.add('movieBody');
    //Hide Movies
    function hideMovies() {
          event.preventDefault();
          removeAllChildNodes(bodyDiv);
          console.log(movies);
        }  
    // find
    function addLike(value) {
        for (var i=0;i<movies.length;i+=1) {
            if (movies[i].DateID === value) {
                movies[i].likes += 1
                console.log("Likes: " + movies[i].likes)
            }
            else {
                console.log("no")
            }
        }
    }
    function Addmovie() {
        var movieindex = movies.length - 1;
        console.log(movies[movieindex].ftitle);
        var image = document.createElement('img');
        image.src = 'images/like.png';
        const clone = image.cloneNode(true);
        const newDiv = document.createElement("div");
        newDiv.innerText = "Movie: " + movies[movieindex].ftitle + "\nComments: " + movies[movieindex].commentary + "\nLikes: " + movies[movieindex].likes + "\nClick to Like: "
        newDiv.appendChild(clone);
        bodyDiv.appendChild(newDiv);
        newDiv.addEventListener("click", function () {
         addLike(movies[movieindex].DateID);
        }); 
    }
    //Displaying Movies
    for (var i=0;i<movies.length;i+=1) {
        console.log(movies[i].ftitle); // shows each movie displayed in console
        var image = document.createElement('img');
        image.src = 'images/like.png';
        const clone = image.cloneNode(true);
        const newDiv = document.createElement("div");
        newDiv.innerText = "Movie: " + movies[i].ftitle + "\nComments: " + movies[i].commentary + "\n Likes: " + movies[i].likes + "\nClick to Like: "
        newDiv.appendChild(clone);
        bodyDiv.appendChild(newDiv);
        newDiv.addEventListener("click", function () {
         addLike(movies[i].DateID);
        }); 
    }
    function sortMovies(array, key) {
            event.preventDefault();
            return array.sort((a, b) => {
              const movieA = a[key].toUpperCase();
              const movieB = b[key].toUpperCase();
              if (movieA < movieB) {
                return -1;
              }
              if (movieA > movieB) {
                return 1;
              }
              return 0;
            });
          }
        function removeAllChildNodes(parent) {
            event.preventDefault();
            while (parent.firstChild) {
            parent.removeChild(parent.firstChild);
         }
        } 
          function logSort() {
            event.preventDefault();
            hideMovies();    
            // Sort the array of dictionaries by the 'ftitle' 
            var sortedData = sortMovies(movies, 'ftitle');        
            // Display the sorted data in the console
            console.log(sortedData);  
            const titleDiv = document.createElement("div");
                titleDiv.classList.add('sortTitle'); 
                titleDiv.innerText = "Sorted Movies Displayed Below:"
                bodyDiv.appendChild(titleDiv);
            for (var i=0;i<movies.length;i+=1) {
                  console.log(movies[i].ftitle); // shows each movie displayed in console
                var image = document.createElement('img');
                image.src = 'images/like.png';
                const clone = image.cloneNode(true);
                const sortDiv = document.createElement("div");
                sortDiv.innerText = "Movie: " + movies[i].ftitle + "\nComments: " + movies[i].commentary + "\n Likes: " + movies[i].likes + "\nClick to Like: "
                sortDiv.appendChild(clone)
                bodyDiv.appendChild(sortDiv)
                sortDiv.addEventListener("click", function () {
                addLike(movies[i].DateID);
                }); 
              }
            }
</script>
<script>
    const url = "https://kkcbal.duckdns.org/api/movies" //replace with api link
    const create_fetch = url + '/create';
    const read_fetch = url + '/';
    read_movie()
    //
    function read_movie() {
        // prepare fetch options
        const read_options = {
            method: 'GET', // *GET, POST, PUT, DELETE, etc.
            mode: 'cors', // no-cors, *cors, same-origin
            cache: 'default', // *default, no-cache, reload, force-cache, only-if-cached
            credentials: 'omit', // include, *same-origin, omit
            headers: {
                'Content-Type': 'application/json'
            },
        };
        // fetch the data from API
        fetch(read_fetch, read_options)
            // response is a RESTful "promise" on any successful fetch
            .then(response => {
            // check for response errors
                if (response.status !== 200) {
                    const errorMsg = 'Database read error: ' + response.status;
                    console.log(errorMsg);
                    newDiv.innerHTML = errorMsg;
                    bodyDiv.appendChild(newDiv)
                    return;
                }
            // valid response will have json data
                response.json().then(data => {
                    console.log(data);
                    //data.sort(function(a, b){return a.time - b.time})
                    //console.log(data);
                    for (let row in data) {
                        console.log(data[row]);
                        movies.push(data[row])
                        add_row(data[row]);
                    }
                })
            })
        // catch fetch errors (ie ACCESS to server blocked)
            .catch(err => {
                console.error(err);
                const newDiv = document.createElement("div");
                newDiv.innerHTML = err;
                bodyDiv.appendChild(newDiv)
            });
    }
    //
    function add_row(data) {
        const newDiv = document.createElement("div");
        // obtain data that is specific to the API
        newDiv.innerHTML = "Movie: " + data.ftitle + "<br>Comments: " + data.commentary
        // add HTML to container
        bodyDiv.appendChild(newDiv)
    }
    //
    function create_movie(){
        const body = {
            DateID: Date.now(),
            ftitle: document.getElementById('ftitle').value,
            commentary: document.getElementById("commentary").value,
            likes: 0
        };
        const requestOptions = {
            method: 'POST',
            mode: 'no-cors',
            body: JSON.stringify(body),
            headers: {
                "content-type": "application/json",
                'Authorization': 'Bearer my-token',
            },
        };
        // URL for Create API
        // Fetch API call to the database to create a new user
        fetch(create_fetch, requestOptions)
            .then(response => {
            // trap error response from Web API
                if (response.status !== 200) {
                    const errorMsg = 'Database create error: ' + response.status;
                    console.log(errorMsg);
<<<<<<< HEAD
                    const newDiv = document.createElement("div");
=======
                    const newDiv = document.createElement("div")
>>>>>>> f691f475c31cb2778764cbfec28ee02ceca17568
                    newDiv.innerHTML = errorMsg;
                    bodyDiv.appendChild(newDiv)
                    return;
                }
            // response contains valid result
                response.json().then(data => {
                    console.log(data);
                    //add a table row for the new/created userid
                    add_row(data);
                })
            })
    }
</script>
</body>