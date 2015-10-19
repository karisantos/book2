# UI

Pick one question class and build an exploratory visualization interface for it.
The question class you pick must have at least three variables that can be changed.

## For a given state and ambience, how many businesses are there at each star rating?

<div style="border:1px grey solid; padding:5px;">
    <div><h5>State</h5>
        <input id="arg1" type="text" value="AZ"/>  (NC,AZ, WI, IL)
    </div>
    <div><h5>Ambience</h5>
        <input id="arg2" type="text" value="casual"/> ("intimate", "classy", "hipster", "divey", "touristy", "trendy", "upscale", "casual" )
    </div>
    <div><h5>Sort by Star Rating</h5>
        <input id="arg3" type="text" value="asc"/>
    </div>    
    <div style="margin:20px;">
        <button id="viz">Vizualize</button>
    </div>
</div>

<div class="myviz" style="width:100%; height:500px; border: 1px black solid; padding: 5px;">
Data is not loaded yet
</div>

{% script %}
items = 'not loaded yet'

console.log('lodash version:', _.VERSION)

$.get('http://bigdatahci2015.github.io/data/yelp/yelp_academic_dataset_business.5000.json.lines.txt')
    .success(function(data){        
        var lines = data.trim().split('\n')

        // convert text lines to json arrays and save them in `items`
        items = _.map(lines, JSON.parse)

        console.log('number of items loaded:', items.length)

        console.log('first item', items[0])
     })
     .error(function(e){
         console.error(e)
     })

function viz(state, ambience, sort){  

  //  var state = "NC";
  //  var ambience = "romantic";
  //  var sort = "asc"

    filtered =  _.filter(items, function(d){
        return ( d.state == state && d.attributes && d.attributes.Ambience && d.attributes.Ambience[ambience] )
        }) 
    console.log('first filter', filtered[0]);
    console.log('filtered length', _.size(filtered))
    var groups = _.groupBy(filtered, "stars")
    console.log('groups', groups)
    var pairs = _.pairs(groups)
    console.log('pairs', pairs)
        
    // TODO: sort pairs in the order specified by users
     var sorted = [];
    //if ((sort == 'asc')||(sort == 'ascending')) {
    if (_.contains(sort, 'asc')) {
        sorted = _.sortBy(pairs, function(d){
                return d[0];
        });
    }else {
        sorted = _(_.sortBy(pairs, function(d){
                return d[0];
        })).reverse().value();
    }

    // define a template string
    var tplString = '<g transform="translate(0 ${d.y})"> \
                    <text y="20">${d.label}</text> \
                    <rect x="30"   \
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
        return d[1].length*5;
    }

    function computeY(d, i) {
        return i * 20
    }

    function computeColor(d, i) {
        return 'red'
    }

    function computeLabel(d, i) {
        return d[0]
    }

    

    var viz = _.map(sorted, function(d, i){                
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

    $('.myviz').html('<svg width="100%" height="100%">' + result + '</svg>')
}

$('button#viz').click(function(){    
    var arg1 = $('input#arg1').val();
    var arg2 = $('input#arg2').val();
    var arg3 = $('input#arg3').val();
    viz(arg1, arg2, arg3)
})  

{% endscript %}

# Authors

This UI is developed by
* [Kari Santos](https://github.com/karisantos)
* [Heather Witte](https://github.com/hswitte)
* [Zachary Lamb](https://github.com/ZachLamb)
* [Fadhil Suhendi](https://github.com/fadhilfath)
* [Denis Kazakov](https://github.com/94kazakov)
