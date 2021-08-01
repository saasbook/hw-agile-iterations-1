To render the maps, this project uses a combination of Javascript and Topojson.
[Topojson](https://github.com/topojson/topojson-specification) is a compact alternative to [GeoJSON](https://tools.ietf.org/html/rfc7946).
These file formats are both used to represent simple geographical features.

Inside the `lib/tasks` folder you will note that there are rake tasks that fetch the
GeoJSON files from `census.gov` and convert them to Topojson. You do not need to run
the rake tasks as this project ships with pre-fetched and pre-built Topojson files.

You will find the Topojson files in `assets/topojson`. They are served via the `assets pipeline` using by `Sprockets`.
On the browser, we need Javascript in addition to Topojson files to render maps as SVG.
To do so, we use `d3.js` library.

This library is fetched using `yarn` from `npmjs.com`. NPM is to Javascript as RubyGems is to Ruby (or Maven is to Java).
In addition, we use Webpack to compile Javascript, SCSS, CSS and their dependencies.
You will find the Javascript that renders the browser in `app/javascript/packs`.

For those of you interested in an extra challenge (there will be no extra credit though),
figure out how to download shapefiles for Congressional Districts
from `census.gov` and create a page that renders the map for Congressional Districts. You may
find [bl.ocks.org](https://bl.ocks.org/) useful.

We also use JQuery to listen and react to DOM events eg. when a form value changes.
Open the `localhost:3000/events` path on your browser and assess how the `Filter By` form works.
What Javascript file handles the changes in the form?

Most fully-fledged applications will need a substantial amount of Javascript to manage state.
While JQuery would suffice for this project, you may later encounter frameworks like React, Vue and Angular
that are more suited for managing state on a page. For medium-sized Rails applications in particular,
[StimulusJS](https://stimulusjs.org) is a great option. For your own practice with Javascript, you may, for example,
re-write the pre-existing Javascript code using StimulusJS (there will be no extra credit for this though).

Next, read the [Iteration Instructions](../checkpoint2/iteration_instructions.md) you'll be following for future checkpoints. 
