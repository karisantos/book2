# Pokemon

Create interactive visualizations for the Pokemon data. Complete the script
so that when a user clicks on each menu button, there will be a different
bar chart shown in the Viz block.

You will be reusing the code you wrote for generating SVG bar charts a couple weeks
ago. The main challenge is to figure out how to connect your visualization code
with the front-end code (event handlers ... etc).

## Menu

<button id="viz-horizontal">Attack (Horizontal Bars)</button>
<button id="viz-vertical">Attack (Vertical Bars)</button>
<button id="viz-attack-defense">Attack vs. Defense</button>
<button id="viz-speed-defense">Speed vs. Defense</button>
<button id="viz-horizontal-sorted">Attack (sorted from low to high)</button>
<button id="viz-horizontal-sorted-desc">Attack (sorted from high to low)</button>
<button id="viz-attack-speed">Attack (width) vs. Speed (color)</button>

## Viz

<div class="myviz" style="width:100%; height:500px; border: 1px black solid;">
Data is not loaded yet
</div>

{% script %}
// pokemonData is a global variable
pokemonData = 'not loaded yet'

$.get('/data/pokemon-small.json')
 .success(function(data){
     console.log('data loaded', data)
     // TODO: show in the myviz that the data is loaded
     pokemonData = data          
 })


function vizAsHorizontalBars(){

    //  visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack;
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-horizontal').click(vizAsHorizontalBars)


// for each of the TODOs below, you will write a "callback function" similar
// to vizAsHorizontalBars and a $('something').click to add this callback
// function to respond to the click event of a particular button

// TODO: add code to visualize the attack points as a series
// of vertical bars (without labels)
function vizAttackAsVerticalBars(){
    // define a template string

    var vizName = "myviz"; // needed to figure height of div containing vizual
    var tplString = '<g transform="translate(${d.x} ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="${d.height}"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString);

    function computeHeight(d, i, data, vizName) {
        /*//var height = $(vizName).height();
        var height = 500;
        var maxObj = _.max(data,function(d){
            return d.Attack;
        });
        var scale = height / maxObj.Attack;
        return d.Attack * scale;
        */

        return d.Attack;
    }
    function computeX(d, i) {
        return i * 20
    }
    function computeWidth(d, i) {
        return 20
    }
    function computeY(d, i,data,vizName) {
        
    //var height = $(vizName).height();
        var height = 500;
    /*    var maxObj = _.max(data,function(d){
            return d.Attack;
        });
        var scale = height / maxObj.Attack;
        return height - (d.Attack * scale); */
        return height - d.Attack;
    }
    function computeColor(d, i) {
        return 'red'
    }

    var viz = _.map(pokemonData, function(d, i, data){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i,pokemonData,vizName ),
                    width: computeWidth(d, i),
                    height: computeHeight(d, i,pokemonData,vizName ),
                    color: computeColor(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg width="600" height="500" >' + result + '</svg>')



}

$('button#viz-vertical').click(vizAttackAsVerticalBars)

// visualize the attack points vs. defense
// points as side-by-side horizontal bar charts (with labels)

function vizAsAttackVsDefense(){
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
    <rect \
         x="-${d.attackWidth}" \
         width="${d.attackWidth}" \
         height="20" \
         style="fill:${d.attackColor}; \
                stroke-width:1; \
                stroke:rgb(0,0,0)" /> \
    <rect \
         x="0" \
         width="${d.defenseWidth}" \
         height="20" \
         style="fill:${d.defenseColor}; \
                stroke-width:1; \
                stroke:rgb(0,0,0)" /> \
    <text transform="translate(0 15)">${d.label}</text> \
</g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
    return 0
    }

    function computeAttackWidth(d, i) {
         return d.Attack;
    }
    function computeDefenseWidth(d, i) {
         return d.Defense;
    }
    function computeY(d, i) {
        return i * 20
    }

    function computeAttackColor(d, i) {
        return 'red'
    }
    function computeDefenseColor(d, i) {
        return 'blue'
    }
    function computeLabel(d) {
        return d.Name
    }
    var viz = _.map(pokemonData, function(d, i){
                return {
                x: computeX(d, i),
                y: computeY(d, i),
                attackWidth: computeAttackWidth(d, i),
                defenseWidth: computeDefenseWidth(d, i),
                attackColor: computeAttackColor(d, i),
                defenseColor: computeDefenseColor(d, i),
                label: computeLabel(d)
            }
    })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-attack-defense').click(vizAsAttackVsDefense)

// TODO: add code visualize the speed points vs. defense
// points as side-by-side horizontal bar charts (with labels)
function vizAsSpeedVsDefense(){
    // define a template string
    var tplString = '<g transform="translate(120 ${d.y})"> \
    <rect \
         x="-${d.speedWidth}" \
         width="${d.speedWidth}" \
         height="20" \
         style="fill:${d.speedColor}; \
                stroke-width:1; \
                stroke:rgb(0,0,0)" /> \
    <rect \
         x="0" \
         width="${d.defenseWidth}" \
         height="20" \
         style="fill:${d.defenseColor}; \
                stroke-width:1; \
                stroke:rgb(0,0,0)" /> \
    <text transform="translate(0 15)">${d.label}</text> \
</g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
    return 0
}

    function computeSpeedWidth(d, i) {
         return d.Speed;
    }
    function computeDefenseWidth(d, i) {
         return d.Defense;
    }
    function computeY(d, i) {
        return i * 20
    }

    function computeSpeedColor(d, i) {
        return 'red'
    }
    function computeDefenseColor(d, i) {
        return 'blue'
    }
    function computeLabel(d) {
        return d.Name
    }
    var viz = _.map(pokemonData, function(d, i){
                return {
                x: computeX(d, i),
                y: computeY(d, i),
                defenseWidth: computeDefenseWidth(d, i),
                speedWidth: computeSpeedWidth(d, i),
                speedColor: computeSpeedColor(d, i),
                defenseColor: computeDefenseColor(d, i),
                label: computeLabel(d)
            }
    })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-speed-defense').click(vizAsSpeedVsDefense)

// visualize the attack points in ascending order as a
// series of horizontal bar charts (with labels)
function vizAsHorizontalBarsSorted(){

    //  visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                        <text transform="translate(0 15)">${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack;
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }
    function computeLabel(d, i) {
        return d.Name
    }

    // sort the pokemonData
    var sortPokemon = _.sortBy(pokemonData, 'Attack')

    var viz = _.map(sortPokemon, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-horizontal-sorted').click(vizAsHorizontalBarsSorted)


// TODO: add code to visualize the attack points in descending order as a
// series of horizontal bar charts (with labels)
function vizAsHorizontalBarsSortedDesc(){

    //  visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0)" />   \
                        <text transform="translate(0 15)">${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack;
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }
    function computeLabel(d, i) {
        return d.Name
    }

    // sort the pokemonData
    var sortPokemon = _(_.sortBy(pokemonData, 'Attack')).reverse().value();

    var viz = _.map(sortPokemon, function(d, i){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-horizontal-sorted-desc').click(vizAsHorizontalBarsSortedDesc)
// TODO: add code to visualize the attack points as a series of horizontal bar
// charts (with labels), and using the brightness of red to represent speed
// points
function vizAsHorizontalAttackSpeedColor(){

    //  visualize the data as horizontal
    // bars to compare attack points

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <rect   \
                         width="${d.width}" \
                         height="20"    \
                         style="fill:${d.color};    \
                                stroke-width:3; \
                                stroke:rgb(0,0,0); \
                                opacity:${d.opacity}"/>   \
                        <text transform="translate(0 15)">${d.label}</text> \
                    </g>'

    // compile the string to get a template function
    var template = _.template(tplString)

    function computeX(d, i) {
        return 0
    }

    function computeWidth(d, i) {
        return d.Attack;
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }
    function computeLabel(d, i) {
        return d.Name
    }
    function computeOpacity(d, i,data) {
        var maxObj = _.max(data,function(d){
            return d.Speed;
        });
        return d.Speed / maxObj.Speed;
    }
    

    var viz = _.map(pokemonData, function(d, i,data){
                return {
                    x: computeX(d, i),
                    y: computeY(d, i),
                    width: computeWidth(d, i),
                    color: computeColor(d, i),
                    label: computeLabel(d, i),
                    opacity: computeOpacity(d, i,data)
                }
             })
    console.log('viz', viz)

    var result = _.map(viz, function(d){
             // invoke the compiled template function on each viz data
             return template({d: d})
         })
    console.log('result', result)

    $('.myviz').html('<svg>' + result + '</svg>')
}

$('button#viz-attack-speed').click(vizAsHorizontalAttackSpeedColor)
{% endscript %}
