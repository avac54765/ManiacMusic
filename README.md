## Maniac Music Project
> This is the frontend repository for Ava and Alexa Carlson's APCSP project.
- Known as "The Carlson Twins" or "X and V" (thank you Mr. Mortensen), we are a power duo that kind of enjoys computer science.
- This project focuses on music. Users are able to add their favorite songs at the moment and look for concerts, comedy shows, and sports games. Users are also able to log into their Spotify to access what playlists and songs they have.

### [Deployed Backend](http://maniacmusic.duckdns.org/)
### [Backend Repository](https://github.com/avac54765/maniacflask) and [Deployed Site](https://avac54765.github.io/ManiacMusic/)

Both of the repositories are templates, so feel free to use the templates to create your own repositories with our code. 
This repository contains edits of the leuck_reunion repository by jm1021.

Clone both repositories to get the full Carlson experience of full stack.

---
For local host:

In a terminal run: 
- bundle install
- bundle exec jekyll serve
- open the link provided

!!! in all nav bars in the includes: change the html table to not include the repo name in the links !!!

---
example:
for deployed (notice the /ManiacMusic/)
<table>
    <tr>
        <td><a href="/ManiacMusic/">Home</a></td>
        <td><a href="/ManiacMusic/eventsapi.html">Events</a></td>
        <td><a href="/ManiacMusic/songs.html">Spotify</a></td>
        <td><a href="/ManiacMusic/favorites.html">Your Favorite Songs!</a></td>
        
     
    
    </tr>
</table>  

for local host (notice the removed repo name):
<table>
    <tr>
        <td><a href="/">Home</a></td>
        <td><a href="/eventsapi.html">Events</a></td>
        <td><a href="/songs.html">Spotify</a></td>
        <td><a href="/favorites.html">Your Favorite Songs!</a></td>
    </tr>
</table> 
