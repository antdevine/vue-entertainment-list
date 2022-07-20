<template>
  <main>
    <form id="category">
      <h3>Categories</h3>
      <div v-for="(category, index) in this.categoriesOptions" :key="index">
        <input type="radio" :value="category" :id="category" name="category" v-model="entertainmentCategory" required /> {{ category }}
      </div>
    </form>

    <form id="search">
      <label for="seach">Search:</label>
      <input type="text" id="search" name="seach" v-model="searchInput"><br><br>
    </form>

    <form id="ratings">
      <h3>Ratings</h3>
      <div v-for="(rating, index) in this.ratingsOptions" :key="index">
        <input type="radio" :value="rating" :id="rating" name="rating" v-model="entertainmentRating" required /> {{ rating }}
      </div>
    </form>

    <input type="button" value="Clear filters and Search" @click="clearFilters" />
    <input type="button" value="Submit filter" @click="filterSelection" />

    <input type="button" @click="toggleModal" value="View your favourites">

    <div id="myModal" class="modal" v-show="modal">
      <div class="modal-content">
        <span class="close" @click="toggleModal">&times;</span>
        
        <FavouritesList v-for="(favourite, index) in this.favourites" :key="index" 
          :favouriteId="index"
          :favouriteTitle="favourite.title"
          @removeFavourites="removeFromFavourites"
        />
        <p v-if="this.favourites.length == 0">No movies in your favourites, please add some</p>
      </div>
    </div>

    <button @click="sortAscending">
      Sort name by {{nameSort}}
    </button>

      <MovieList v-for="(movie, index) in this.feedData" :key="index"
        :movieid="index" 
        :movieTitle="movie.title" 
        :movieImg="movie.thumbnail.regular.medium"
        @addFavourites="addToFavourites" 
      />
    
    <p v-if="this.feedData.length == 0">No movies in this filter, please clear filter and try again</p>
  </main>
</template>

<script>
import { RouterLink, RouterView } from 'vue-router';
import MovieList from '@/components/MovieList.vue';
import FavouritesList from '@/components/FavouritesList.vue';

  export default {
    components: {
      MovieList,
      FavouritesList,
    },
    data() {
      return {
        entertainmentCategory: '',
        entertainmentRating: '',
        movieData: [
          {
            "title": "Beyond Earth",
            "thumbnail": {
              "trending": {
                "small": "./src/assets/thumbnails/beyond-earth/trending/small.jpg",
                "large": "./src/assets/thumbnails/beyond-earth/trending/large.jpg"
              },
              "regular": {
                "small": "./src/assets/thumbnails/beyond-earth/regular/small.jpg",
                "medium": "./src/assets/thumbnails/beyond-earth/regular/medium.jpg",
                "large": "./src/assets/thumbnails/beyond-earth/regular/large.jpg"
              }
            },
            "year": 2019,
            "category": "Movie",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": true
          },
          {
            "title": "Bottom Gear",
            "thumbnail": {
              "trending": {
                "small": "./src/assets/thumbnails/bottom-gear/trending/small.jpg",
                "large": "./src/assets/thumbnails/bottom-gear/trending/large.jpg"
              },
              "regular": {
                "small": "./src/assets/thumbnails/bottom-gear/regular/small.jpg",
                "medium": "./src/assets/thumbnails/bottom-gear/regular/medium.jpg",
                "large": "./src/assets/thumbnails/bottom-gear/regular/large.jpg"
              }
            },
            "year": 2021,
            "category": "Movie",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": true
          },
          {
            "title": "Undiscovered Cities",
            "thumbnail": {
              "trending": {
                "small": "./src/assets/thumbnails/undiscovered-cities/trending/small.jpg",
                "large": "./src/assets/thumbnails/undiscovered-cities/trending/large.jpg"
              },
              "regular": {
                "small": "./src/assets/thumbnails/undiscovered-cities/regular/small.jpg",
                "medium": "./src/assets/thumbnails/undiscovered-cities/regular/medium.jpg",
                "large": "./src/assets/thumbnails/undiscovered-cities/regular/large.jpg"
              }
            },
            "year": 2019,
            "category": "TV Series",
            "rating": "E",
            "isBookmarked": false,
            "isTrending": true
          },
          {
            "title": "1998",
            "thumbnail": {
              "trending": {
                "small": "./src/assets/thumbnails/1998/trending/small.jpg",
                "large": "./src/assets/thumbnails/1998/trending/large.jpg"
              },
              "regular": {
                "small": "./src/assets/thumbnails/1998/regular/small.jpg",
                "medium": "./src/assets/thumbnails/1998/regular/medium.jpg",
                "large": "./src/assets/thumbnails/1998/regular/large.jpg"
              }
            },
            "year": 2021,
            "category": "Movie",
            "rating": "18+",
            "isBookmarked": false,
            "isTrending": true
          },
          {
            "title": "Dark Side of the Moon",
            "thumbnail": {
              "trending": {
                "small": "./src/assets/thumbnails/dark-side-of-the-moon/trending/small.jpg",
                "large": "./src/assets/thumbnails/dark-side-of-the-moon/trending/large.jpg"
              },
              "regular": {
                "small": "./src/assets/thumbnails/dark-side-of-the-moon/regular/small.jpg",
                "medium": "./src/assets/thumbnails/dark-side-of-the-moon/regular/medium.jpg",
                "large": "./src/assets/thumbnails/dark-side-of-the-moon/regular/large.jpg"
              }
            },
            "year": 2018,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": true,
            "isTrending": true
          },
          {
            "title": "The Great Lands",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/the-great-lands/regular/small.jpg",
                "medium": "./src/assets/thumbnails/the-great-lands/regular/medium.jpg",
                "large": "./src/assets/thumbnails/the-great-lands/regular/large.jpg"
              }
            },
            "year": 2019,
            "category": "Movie",
            "rating": "E",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "The Diary",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/the-diary/regular/small.jpg",
                "medium": "./src/assets/thumbnails/the-diary/regular/medium.jpg",
                "large": "./src/assets/thumbnails/the-diary/regular/large.jpg"
              }
            },
            "year": 2019,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Earth’s Untouched",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/earths-untouched/regular/small.jpg",
                "medium": "./src/assets/thumbnails/earths-untouched/regular/medium.jpg",
                "large": "./src/assets/thumbnails/earths-untouched/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "18+",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "No Land Beyond",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/no-land-beyond/regular/small.jpg",
                "medium": "./src/assets/thumbnails/no-land-beyond/regular/medium.jpg",
                "large": "./src/assets/thumbnails/no-land-beyond/regular/large.jpg"
              }
            },
            "year": 2019,
            "category": "Movie",
            "rating": "E",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "During the Hunt",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/during-the-hunt/regular/small.jpg",
                "medium": "./src/assets/thumbnails/during-the-hunt/regular/medium.jpg",
                "large": "./src/assets/thumbnails/during-the-hunt/regular/large.jpg"
              }
            },
            "year": 2016,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Autosport the Series",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/autosport-the-series/regular/small.jpg",
                "medium": "./src/assets/thumbnails/autosport-the-series/regular/medium.jpg",
                "large": "./src/assets/thumbnails/autosport-the-series/regular/large.jpg"
              }
            },
            "year": 2016,
            "category": "TV Series",
            "rating": "18+",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Same Answer II",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/same-answer-2/regular/small.jpg",
                "medium": "./src/assets/thumbnails/same-answer-2/regular/medium.jpg",
                "large": "./src/assets/thumbnails/same-answer-2/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "E",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Below Echo",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/below-echo/regular/small.jpg",
                "medium": "./src/assets/thumbnails/below-echo/regular/medium.jpg",
                "large": "./src/assets/thumbnails/below-echo/regular/large.jpg"
              }
            },
            "year": 2016,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "The Rockies",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/the-rockies/regular/small.jpg",
                "medium": "./src/assets/thumbnails/the-rockies/regular/medium.jpg",
                "large": "./src/assets/thumbnails/the-rockies/regular/large.jpg"
              }
            },
            "year": 2015,
            "category": "TV Series",
            "rating": "E",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Relentless",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/relentless/regular/small.jpg",
                "medium": "./src/assets/thumbnails/relentless/regular/medium.jpg",
                "large": "./src/assets/thumbnails/relentless/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "PG",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Community of Ours",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/community-of-ours/regular/small.jpg",
                "medium": "./src/assets/thumbnails/community-of-ours/regular/medium.jpg",
                "large": "./src/assets/thumbnails/community-of-ours/regular/large.jpg"
              }
            },
            "year": 2018,
            "category": "TV Series",
            "rating": "18+",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Van Life",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/van-life/regular/small.jpg",
                "medium": "./src/assets/thumbnails/van-life/regular/medium.jpg",
                "large": "./src/assets/thumbnails/van-life/regular/large.jpg"
              }
            },
            "year": 2015,
            "category": "Movie",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "The Heiress",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/the-heiress/regular/small.jpg",
                "medium": "./src/assets/thumbnails/the-heiress/regular/medium.jpg",
                "large": "./src/assets/thumbnails/the-heiress/regular/large.jpg"
              }
            },
            "year": 2021,
            "category": "Movie",
            "rating": "PG",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Off the Track",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/off-the-track/regular/small.jpg",
                "medium": "./src/assets/thumbnails/off-the-track/regular/medium.jpg",
                "large": "./src/assets/thumbnails/off-the-track/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "18+",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Whispering Hill",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/whispering-hill/regular/small.jpg",
                "medium": "./src/assets/thumbnails/whispering-hill/regular/medium.jpg",
                "large": "./src/assets/thumbnails/whispering-hill/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "E",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "112",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/112/regular/small.jpg",
                "medium": "./src/assets/thumbnails/112/regular/medium.jpg",
                "large": "./src/assets/thumbnails/112/regular/large.jpg"
              }
            },
            "year": 2013,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Lone Heart",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/lone-heart/regular/small.jpg",
                "medium": "./src/assets/thumbnails/lone-heart/regular/medium.jpg",
                "large": "./src/assets/thumbnails/lone-heart/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "E",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Production Line",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/production-line/regular/small.jpg",
                "medium": "./src/assets/thumbnails/production-line/regular/medium.jpg",
                "large": "./src/assets/thumbnails/production-line/regular/large.jpg"
              }
            },
            "year": 2018,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Dogs",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/dogs/regular/small.jpg",
                "medium": "./src/assets/thumbnails/dogs/regular/medium.jpg",
                "large": "./src/assets/thumbnails/dogs/regular/large.jpg"
              }
            },
            "year": 2016,
            "category": "TV Series",
            "rating": "E",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Asia in 24 Days",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/asia-in-24-days/regular/small.jpg",
                "medium": "./src/assets/thumbnails/asia-in-24-days/regular/medium.jpg",
                "large": "./src/assets/thumbnails/asia-in-24-days/regular/large.jpg"
              }
            },
            "year": 2020,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "The Tasty Tour",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/the-tasty-tour/regular/small.jpg",
                "medium": "./src/assets/thumbnails/the-tasty-tour/regular/medium.jpg",
                "large": "./src/assets/thumbnails/the-tasty-tour/regular/large.jpg"
              }
            },
            "year": 2016,
            "category": "TV Series",
            "rating": "PG",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Darker",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/darker/regular/small.jpg",
                "medium": "./src/assets/thumbnails/darker/regular/medium.jpg",
                "large": "./src/assets/thumbnails/darker/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "18+",
            "isBookmarked": true,
            "isTrending": false
          },
          {
            "title": "Unresolved Cases",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/unresolved-cases/regular/small.jpg",
                "medium": "./src/assets/thumbnails/unresolved-cases/regular/medium.jpg",
                "large": "./src/assets/thumbnails/unresolved-cases/regular/large.jpg"
              }
            },
            "year": 2018,
            "category": "TV Series",
            "rating": "18+",
            "isBookmarked": false,
            "isTrending": false
          },
          {
            "title": "Mission: Saturn",
            "thumbnail": {
              "regular": {
                "small": "./src/assets/thumbnails/mission-saturn/regular/small.jpg",
                "medium": "./src/assets/thumbnails/mission-saturn/regular/medium.jpg",
                "large": "./src/assets/thumbnails/mission-saturn/regular/large.jpg"
              }
            },
            "year": 2017,
            "category": "Movie",
            "rating": "PG",
            "isBookmarked": true,
            "isTrending": false
          }
        ],
        categoriesOptions: [],
        ratingsOptions: [],
        preFilter: [],
        feedData: [],
        searchInput: '',
        favourites: [],
        modal: false,
        nameSort: "Ascending",
      }
    },
    methods: {
      categoriesList() {
        let categories = this.movieData.map((movie) => movie.category);
        this.categoriesOptions = [...new Set(categories)];
      },
      ratingsList() {
        let ratings = this.movieData.map((movie) => movie.rating);
        this.ratingsOptions = [...new Set(ratings)];
      },
      filterSelection() {
        this.resetFeed();

        const categorySelection = this.movieData.filter(movie => {
          return movie.category === this.entertainmentCategory;
        });

        const ratingSelection = this.movieData.filter(movie => {
          return movie.rating === this.entertainmentRating;
        });

        const joinFilters = [...categorySelection, ...ratingSelection];
        const noDuplicates = joinFilters.length ? joinFilters : this.movieData;
        const duplicatesFilters = joinFilters => joinFilters.filter((item, index) => (joinFilters.indexOf(item) !== index));
        this.feedData = duplicatesFilters(joinFilters).length ? duplicatesFilters(joinFilters) : noDuplicates;

        if (this.searchInput.length) {
          const searchFilter = this.feedData.filter((movie) => {
              return movie.title
              .toUpperCase()
              .includes(this.searchInput.toUpperCase())
             });
             this.feedData = searchFilter;
        }
      },
      resetFeed() {
        this.feedData = this.movieData;
      },
      clearFilters() {
        this.feedData = this.movieData;
        this.entertainmentCategory = '';
        this.entertainmentRating = '';
        this.searchInput = '';
      },
      addToFavourites(id) {
        if (!this.favourites.includes(this.movieData[id])) {
          this.favourites.push(this.movieData[id]);
        }
      },
      removeFromFavourites(title) {
        const filtered = this.favourites.filter(favourite => favourite.title !== title);
        this.favourites = filtered;
      },
      toggleModal() {
        this.modal = !this.modal;
      },
      sortAscending() {
        console.log('were sorting');
        let sortedData = this.movieData;
        if(this.nameSort === "Ascending") {
            sortedData = sortedData.sort((a,b) => {
                let fa = a.title.toLowerCase(), fb = b.title.toLowerCase();
                if (fa < fb) {
                    return -1
                }
                if (fa > fb) {
                    return 1
                }
                
                return 0
            })
            this.nameSort = "Descending"
        }
        else {
            sortedData.reverse();
            this.nameSort = "Ascending"
        }
        this.movieData = sortedData;
      },
    },
    beforeMount() {
      this.resetFeed();
      this.categoriesList();
      this.ratingsList();
    },
  }
</script>

<style scoped>
  .modal {
    position: fixed; /* Stay in place */
    z-index: 1; /* Sit on top */
    left: 0;
    top: 0;
    width: 100%; /* Full width */
    height: 100%; /* Full height */
    overflow: auto; /* Enable scroll if needed */
    background-color: rgb(0,0,0); /* Fallback color */
    background-color: rgba(0,0,0,0.4); /* Black w/ opacity */
  }

  /* Modal Content/Box */
  .modal-content {
    background-color: #fefefe;
    margin: 15% auto; /* 15% from the top and centered */
    padding: 20px;
    border: 1px solid #888;
    width: 80%; /* Could be more or less, depending on screen size */
  }

  /* The Close Button */
  .close {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
    z-index: 999999;
  }

  .close:hover,
  .close:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
  }
</style>