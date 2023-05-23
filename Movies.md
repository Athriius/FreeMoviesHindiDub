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
}
.movieBody {
  margin: auto;
  color: white;
  border: 3px solid  #FFC133;
  padding: 12px;
  width: 1000px;
  background: #f2f2f2;
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
        <button onclick="changeStyle()">Hide Movies</button>
    </form>
    <script>
        let movies = [{id: 1, ftitle: 'Joker', commentary: 'I\'m the joker baby'}];
        // example {id:1592304983049, title: 'Avengers: Endgame', commentary: 'good action scenes.'}
        const addMovie = (ev)=>{
            ev.preventDefault();  //stops the form submitting automatically
            let movie = {
                id: Date.now(),
                ftitle: document.getElementById('ftitle').value,
                commentary: document.getElementById('commentary').value
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
        function Addmovie() {
            var movieindex = movies.length - 1;
            console.log(movies[movieindex].ftitle);
            const newDiv = document.createElement("div");
            newDiv.innerText = "Movie: " + movies[movieindex].ftitle + "\nComments: " + movies[movieindex].commentary
            bodyDiv.appendChild(newDiv)
        }
        const newTitle = document.createElement("H1");
        newTitle.innerText = '\xa0\xa0' + "Displayed below are your movies and commentary"
        document.body.appendChild(newTitle)
        // Creating Body
        var bodyDiv = document.createElement("div");
        document.body.appendChild(bodyDiv);
        bodyDiv.classList.add('movieBody');
        //Displaying Movies
        for (var i=0;i<movies.length;i+=1) {
            console.log(movies[i].ftitle); // shows each movie displayed in console
            const newDiv = document.createElement("div");
            newDiv.innerText = "Movie: " + movies[i].ftitle + "\nComments: " + movies[i].commentary
            bodyDiv.appendChild(newDiv)
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
              function logSort() {
                event.preventDefault();    
                // Sort the array of dictionaries by the 'ftitle' 
                var sortedData = sortMovies(movies, 'ftitle');        
                // Display the sorted data in the console
                console.log(sortedData);  
                const titleDiv = document.createElement("div");
                    titleDiv.classList.add('sortTitle'); 
                    titleDiv.innerText = "Sorted Movies Displayed Below:"
                    document.body.appendChild(titleDiv);
                for (var i=0;i<movies.length;i+=1) {
                     console.log(movies[i].ftitle); // shows each movie displayed in console
                    const sortDiv = document.createElement("div");
                    sortDiv.innerText = "Movie: " + movies[i].ftitle + "\nComments: " + movies[i].commentary
                    document.body.appendChild(sortDiv)
                 }
                }
        function changeStyle() {
          event.preventDefault();
          document.getElementById("bodyDiv").style.display = 'none';
}
    </script>
</body>