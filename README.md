# What to Watch

![Supportive GIF of your App in action](./assets/images/Screenshot%202023-10-16%20191043.png) 
[![DEPLOY](https://img.shields.io/badge/DEPLOY-%E2%9C%93-brightgreen)](https://jamessahunter.github.io/What-to-watch/)

## Technology Used

| Technology         | Resource URL                                    |
| -------------------|:-----------------------------------------------:|
| HTML               | [MDN Web Docs - HTML](https://developer.mozilla.org/en-US/docs/Web/HTML) |
| CSS                | [MDN Web Docs - CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) |
| Tailwind CSS       | [tailwindcss - Official Website](https://tailwindcss.com) |
| Git                | [Git - Official Website](https://git-scm.com/) |
| JavaScript         | [W3Schools - JavaScript](https://www.w3schools.com/js/default.asp) |
| jQuery             | [jQuery API Documentation](https://api.jquery.com/) |
| jQuery UI          | [jQuery UI - Official Website](https://jqueryui.com/) |
| Slick Carousel     | [Slick Carousel on GitHub](https://github.com/kenwheeler/slick) |
| TMDB API           | [TMDB API Documentation](https://developer.themoviedb.org/docs) |
| OMDB API           | [OMDB API](https://www.omdbapi.com/) |
| NYT API            | [New York Times API](https://developer.nytimes.com/apis) |
| Streaming Availability API | [Streaming Availability API](https://rapidapi.com/movie-of-the-night-movie-of-the-night-default/api/streaming-availability) |

## Description

![Usage Example Full Site](/assets/images/gifs/What%20to%20Watch%20full%20use.gif)

"What to Watch" is a web application that simplifies the process of searching for movies and customizing the information displayed according to the user's interests.

The purpose of the project was to allow the user to search for a movie in an input field and have the information that they found relevent shown for the movie that was searched. The previously searched for movies would be stored on the page. The user could check the criteria they were interested in and that would dynamically update the page. The user could save their cirteria configuration until they want to clear it. The user could also find popular movie in general or for a specific genre by selecting the the genre from a drop down. The user could pin movie they wanted to the page so that they would remain persistent even when the page was refreshed. This project used a 3rd party Application User Interface called Slick that created a carousel for the searched movie titles. There were also server side APIs used such as OMDB, TMDB, New York Times and Streaming Availability. These server side APIs were used to get the criteria that is displayed on the page.
The motivation behind this project was to provide users with a comprehensive movie search experience, where they can easily find, review, and pin their desired categories of movie information. By leveraging multiple APIs, such as OMDB, TMDB, New York Times, and Streaming Availability, users can explore detailed movie data and discover where movies are available to stream or rent.

"What to Watch" sets itself apart by offering an intuitive interface with the following key features:

- Dynamic movie card display based on user searches.
- The ability to customize displayed movie information.
- Persistent storage for search history and user configurations.
- Genre-based movie selection from a dropdown menu.
- Pinned movie cards that remain visible even after page refresh.

## Table of Contents

- [Usage](#usage)
- [Technologies Used](#technology-used)
- [User Stories](#user-stories)
- [API Calls](#user-stories-related-to-api-calls)
- [Description of CSS Framework Tailwind](#description-of-css-framework-tailwind)
- [Learning Objectives](#learning-objectives)
- [Authors / Contact](#authors--contact)
- [Credits](#credits)
- [License](#license)

## Usage

On page load, the user is presented with a feature-rich interface divided into two columns:

### Left Column: Search

- Search Input Field and Search Button.
- Popular Movies Button.
- Feature Checkboxes to customize displayed information.
- Genres Dropdown.
- Save Config, Clear Config, Clear Search History, and Clear Unpinned Movies buttons.

![Left Column](./assets/images/left-column.png)

### Popular Movies

The "Popular Movies" button presents users with the current top 20 movies from TMDB. Selecting a genre from the dropdown filters the top 20 movies by genre.

### Right Column: Results

- Search History Carousel.
- Dynamic display of movie cards based on user actions.
- Pinned Movie Cards.

![Overall screenshot](./assets/images/Screenshot%202023-10-16%20191538.png)

### Search History Carousel

Successful search queries are presented in a carousel for easy access and saved to local storage, so they persist across page reloads.

### Movie Cards

Movie cards provide a wealth of information, including posters, release year, MPAA rating, runtime, genre, director, cast, summary, scores, NYT reviews, and streaming availability. Users can customize the information displayed by altering checkbox selections.

### Pinned Movies

Pinned movie cards are stored in local storage, ensuring they also persist across page reloads.

## User Stories

- I want to see my search history somewhere on the page.

After each successful search, the search query is stored in local storage and added to the search history carousel at the top of the results section. Each query term is a clickable link that will redo the search and replace the result in the movie card window.

![Search history carousel](./assets/images/gifs/What%20to%20Watch%20Carousel.gif)

- I want to be able to select the info I care about.

Checkboxes on the left column allow the user to select which categories of information they want to see on the movie cards. An event listener detects any checkbox change, updating an object variable that stores the checkbox state and toggling the display status for the designated category using a class selector. If the user clicks the **Save Config** button, the current checkbox state will be saved to local storage and will persist on page reload. Clicking **Clear Config** will reset the checkboxes to an all-on state.  

![Select criteria](./assets/images/gifs/What%20to%20Watch%20select%20criteria.gif)

- I want to pin certain movies on the page while I’m searching.

When a search query is run, movie data is collected from various API calls (see below) and saved to an object variable. This object variable is then appended to a nested movie list object variable, as additional searches are performed, and this nested object variable is used to append the movie cards to the page.

An event listener on the movie card container uses event delegation to return clicks on individual movie cards. Their "data-pinned" tag is toggled between "true" and "false", and their element index is correleated with the object index in the movie list object variable to update a pinned key there as well. The code then loops through all the currently displayed movie card objects and saves the pinned === true objects into a separate object variable which is also saved to local storage. These pinned movies will be restored on page reload.

![Select criteria](./assets/images/gifs/What%20to%20Watch%20Pinned%20movies.gif)



## User Stories Related to API Calls

- I want to input my search query.
- I want to search for and review side-by-side my desired categories of movie information.

Once the movie has been entered into the search bar and the search button has been clicked or the enter key has been pressed. This calls a function that handles the search and ensure something was enterd into the search box and starts the API calls. First there is a call to the OMDB API using the input of movie. This is a general search for a list of 10 movies containing the searched for movie in the title. The results are then checked to see if there are duplicates of the title in which case the user is prompted through a modal to select the year of the movie they would like to search for. If there are no duplicate or after the year is selected another call is made to OMDB this time searching for the specific title and year. The reponse to this call is then checked to see if there is a matching title if so it parse the data and enters it into an obejct. If there is no match the user is informed through a modal. Then the TMDB API is called with the movie and the year and more data is parsed and added to the movie object. Then the NYT API is called which uses the year and the movie to get the appropriate review and adds it to the movie object. Then the streaming availabiltiy API is called which finds out where the movie is streaming and if it is available to rent, buy etc. The information is then appended to the page with the only the information that the user wants to see shown.

![Usage Example Searc](/assets/images/gifs/What%20to%20Watch%20Search.gif)

- I want to click a button to see popular movies.
- I want to explore movies in various genres.

First the user can select a genre from the drop down. The user can then click the popular movies button. Once the button is clicked a function gets the id of the genre if there is one. Then a call is made to the TMDB API for popular movies. This then iterates through the array of movie and repeats the process above.

![Usage Example](/assets/images/gifs/What%20to%20Watch%20popular.gif)

## API Calls

The user story of searching for desired movie information involves several API calls:

1. An initial call to the OMDB API with the movie title to retrieve a list of relevant movies.
2. Handling duplicates and, if necessary, prompting the user to select the year of the movie.
3. A second call to OMDB with the selected title and year.
4. A call to the TMDB API for additional movie data.
5. Using the NYT API to fetch the appropriate review.
6. Using the Streaming Availability API to check where the movie is available to stream, rent, or buy.

Only the information that the user selects is displayed.

## Description of CSS Framework Tailwind

Tailwind CSS, a utility-first CSS framework, was used to style the "What to Watch" web application. It streamlines the styling process by providing a set of pre-defined classes for consistent and visually appealing user interface components.

You can see an exampe of the responsiveness of the website below.

![Usage Example Mobile Responsive](/assets/images/gifs/What%20to%20Watch%20Mobile%20Responsize.gif)

## Learning Objectives

This project we learned about the git work flow when working with others. We learned about how to read new documentation that we had not been introduced to previously whether that was a new CSS framework, JS library, or Server Side API.

## Authors / Contact

- James Hunter ([GitHub](https://github.com/jamessahunter))
- Fiqre Tezare ([GitHub](https://github.com/Fiqre-Ab))
- David Wright ([GitHub](https://github.com/d-a-v-i-d-w-r-i-g-h-t))

## Credits

### Third-Party Contributions

- Ken Wheeler: Created the Slick carousel that contains the previously searched for movies. [GitHub Profile](https://github.com/kenwheeler)

## License

This project is licensed under the MIT License. For detailed information, see the [LICENSE](LICENSE) file.

MIT License © 2023 James Hunter, David Wright, Fiqre Tezare

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software")...

