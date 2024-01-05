<template>
  <div class="app-container">
    <div id="myholder" ref="myholder">hello</div>
    <div id="myholder-small"></div>
  </div>
</template>

<script>

// all jointjs and backbone dependencies are imported in index.html (they match the versions on emis)

export default {
  name: 'JointJS',
  data() {
    return {}
  },
  mounted() {
    // create the main paper our shapes will reside on
    var graph = new joint.dia.Graph;
    var paper = new joint.dia.Paper({
      el: $('#myholder'),
      width: 1000,
      height: 1000,
      model: graph,
      gridSize: 1
    });
    // preview window at bottom of page
    var paperSmall = new joint.dia.Paper({
      el: $('#myholder-small'),
      width: 600,
      height: 200,
      model: graph,
      gridSize: 1
    });
    paperSmall.scale(.5);
    // make unclickable
    paperSmall.$el.css('pointer-events', 'none');

    // var paperScroller = new joint.ui.PaperScroller({
    //   paper: paper, // IMPORTANT: this is where we let PaperScroller manage the Paper
    //   autoResizePaper: false,
    //   cursor: 'grab', //default cursor on paper to grab
    // });
    // $('#myholder').append(paper); // append to DOM
    // paperScroller.render().center()

    // define two rectangles
    var rect = new joint.shapes.basic.Rect({
      position: {
        x: 100,
        y: 30
      },
      size: {
        width: 100,
        height: 30
      },
      attrs: {
        rect: {
          fill: 'blue'
        },
        text: {
          text: 'my box',
          fill: 'white'
        }
      }
    });
    var rect2 = rect.clone();
    rect2.translate(300);
    rect.attr({
      rect: { fill: '#2C3E50', rx: 5, ry: 5, 'stroke-width': 2, stroke: 'black' },
      text: {
        text: 'my label', fill: '#3498DB',
        'font-size': 18, 'font-weight': 'bold', 'font-variant': 'small-caps', 'text-transform': 'capitalize'
      }
    });

    var link = new joint.dia.Link({
      source: {
        id: rect.id
      },
      target: {
        id: rect2.id
      }
    });
    link.attr({
      '.connection': { stroke: 'blue' },
    });

    // testing link settings
    // link.set('vertices', [{ x: 300, y: 60 }, { x: 400, y: 60 }, { x: 400, y: 20 }])
    // link.set('smooth', true)

    var circle = new joint.shapes.standard.Circle({
      name: 'circle',
      angle: 90,
      z: 10,
      lineType: 'solid',
      position: {
        x: 100,
        y: 300
      },
      attrs: {
        body: {
          refCx: '50%',
          refCy: '50%',
          refR: '50%',
          strokeWidth: 100,
          stroke: '#687436',
          fill: '#fffff',
        },
        label: {
          text: 'circle90degrees',
          textVerticalAnchor: 'middle',
          textAnchor: 'middle',
          refX: '50%',
          refY: '50%',
          fontSize: 14,
          fill: '#fffff',
        },
      },
    })

    var path1 = new joint.shapes.standard.Path({
      name: 'step',
      lineType: 'solid',
      angle: 0,
      z: 1,
      position: {
        x: 300,
        y: 300
      },
      attrs: {
        body: {
          refD: 'M0 0 26 0 32 10 26 20 0 20 6 10Z',
          strokeWidth: 30,
          stroke: '#89eb34',
          fill: '#89eb34',
          refCx: '25%',
          refCy: '25%',
        },
        label: {
          textVerticalAnchor: 'middle',
          textAnchor: 'middle',
          refX: '50%',
          refY: '50%',
          fontSize: 14,
          fill: '#fffff',
          text: 'path1',
        },
      },
    })

    var text = new joint.shapes.standard.TextBlock({
      name: 'text',
      angle: 0,
      z: 1,
      lineType: 'solid',
      size: {
        width: 150,
        height: 15,
      },
      attrs: {
        body: {
          fill: 'rgba(0, 0, 0, 0)',
          stroke: 'rgba(0, 0, 0, 0)',
          strokeWidth: 1,
        },
        label: {
          text: 'Text',
          style: {
            fontSize: 14,
            color: 'rgba(0, 0, 0, 1)',
          },
        },
      },
    });

    var cylinder = new joint.shapes.standard.Cylinder({
      name: 'cylinder',
      angle: 0,
      z: 1,
      position: {
        x: 400,
        y: 400
      },
      lineType: 'solid',
      attrs: {
        top: {
          strokeWidth: 10,
          fillOpacity: 0.5,
        },
        body: {
          strokeWidth: 10,
          fillOpacity: 0.8,
        },
      },
    });

    var clipboard = new joint.ui.Clipboard({ useLocalStorage: false });
    clipboard.clear();

    var backbone = new Backbone.Collection();
    // console.log(backbone)

    // add the components to the graph
    graph.addCells([rect, rect2, link, circle, path1, text, cylinder]);

    // testing event listeners
    graph.on('all', function (eventName, cell) {
    });
    rect.on('change:position', function (element) {
      console.log('position of my label changed');
    });
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1,
h2 {
  font-weight: normal;
}

ul {
  list-style-type: none;
  padding: 0;
}

li {
  display: inline-block;
  margin: 0 10px;
}

a {
  color: #42b983;
}
</style>