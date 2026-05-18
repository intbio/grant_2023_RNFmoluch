### Траектория огрубленной фибриллы без ПТМ гистоновых хвостов.
[Назад](http://intbio.github.io/grant_2023_RNFmoluch/year3)

<html lang="en">
  <head>
    <meta charset="utf-8">
    <style>
      .slidecontainer {
        width: 100%;
      }

      .slider {
        -webkit-appearance: none;
        width: 100%;
        height: 10px;
        background: #d3d3d3;
        outline: none;
        opacity: 0.7;
        -webkit-transition: .2s;
        transition: opacity .2s;
      }

      .slider:hover {
        opacity: 1;
      }

      .slider::-webkit-slider-thumb {
        -webkit-appearance: none;
        appearance: none;
        width: 15px;
        height: 15px;
        background: #222222;
        cursor: pointer;
      }

      .slider::-moz-range-thumb {
        width: 25px;
        height: 25px;
        background: #4CAF50;
        cursor: pointer;
      }

    </style>
  </head>

  <body>
<script src="https://d3js.org/d3.v4.js"></script>
<script src="https://unpkg.com/ngl@2.0.0-dev.35/dist/ngl.js"></script>
<script src="https://code.jquery.com/jquery-3.5.1.min.js" integrity="sha256-9/aliU8dGd2tb6OSsuzixeV4y/faTqgFtohetphbbj0=" crossorigin="anonymous"></script>
<script>
  var pdb="no_ptm_13.pqr";
  var xtc="no_ptm_13.xtc";
  var trjstep = 4000;
  var player = new NGL.TrajectoryPlayer(window.traj, {...});
  window.traj.player = player;
  $(document).ready(function() {
    window.stage = new NGL.Stage("viewport0", {
      backgroundColor: "#FFFFFF"
    });
    window.stage.loadFile(pdb).then(function(nucl) {
      var aspectRatio = 2;
      var radius = 1.5;
      var radiusScale = 4;


      var hyper_scheme = NGL.ColormakerRegistry.addSelectionScheme([
        ["orange", ".CA"],
        ["blue", "_N"],
        ["red", "_O"],
        ["grey", "*"]
      ], "DA");
      var residues = NGL.ColormakerRegistry.addSelectionScheme([
        ["blue", "40 and ARG"],
	["lightcyan", "42 and ARG"],
	["cyan", "49 and ARG"],
        ["green", "41 and TYR"],
	["pink", "39 and HIS"],
        ["white", "*"]
      ], "protein");
      
      var shape = new NGL.Shape( "Axes" );
			shape.addArrow( [ 0, 0, -35 ], [ 0, 0, 35 ], [ 0.04, 0.8, 0.03 ], 2.0 );
      shape.addArrow( [ -58, 0, 0 ], [ 58, 0, 0 ], [ 0.8,0.06 ,0.102  ], 2.0 );
      shape.addArrow( [ 0, -55, 0 ], [ 0, 55, 0 ], [ 0.3, 0.3, 0.3 ], 2.0 );
      window.axes = stage.addComponentFromObject( shape );
      window.axes.addRepresentation( "buffer" );
      window.axes.autoView();
      window.axes.setVisibility(false);
      stage.animationControls.rotate([ 0, 1, 0, 0 ],0);
	  stage.setParameters({cameraType: "orthographic"});


      nucl.addRepresentation("spacefill", {
        sele: ":P",
        colorScheme: "partialCharge",
        radiusType: "explicit"
      });
      nucl.addRepresentation("spacefill", {
        sele: ":D",
        colorScheme: "partialCharge",
        radiusType: "explicit"
      });

      NGL.autoLoad(xtc).then(function(frames) {
        nucl.addTrajectory(frames);
        window.traj = nucl.trajList[0].trajectory;
        var player = new NGL.TrajectoryPlayer(window.traj, {
          start: 0,
          timeout: 1,
          mode: "once",
          interpolateType: "spline",
          step: 1,
          interpolateStep: 5
        });
        window.traj.signals.frameChanged.add(function() {
          var fnum = window.traj.currentFrame;
          $('#myRange')[0].value = fnum;
          $("#frame_counter")[0].innerHTML = (fnum * trjstep);
          tooltipLine.attr('stroke', 'black')
            .attr('x1', x(fnum))
            .attr('x2', x(fnum))
            .attr('y1', 0)
            .attr('y2', height);
        });



        //window.player.play();
        $('#myRange')[0].setAttribute('max', window.traj.frames.length - 1)

      });
      nucl.autoView();

    });
    

    var slider = document.getElementById("myRange");
    var output = document.getElementById("frame_counter");
    output.innerHTML = slider.value;
    window.slider = slider;

    slider.oninput = function() {
      //output.innerHTML = (this.value*trjstep).toFixed(2);
      window.traj.player.pause();
      window.traj.setFrame(this.value);
      //tooltipLine.attr('stroke', 'black')
      //  .attr('x1', x(this.value))
      //  .attr('x2', x(this.value))
      //  .attr('y1', 0)
      //  .attr('y2', height);
    }
    


    function load_reference_structure() {
      window.ref_str_mol = window.stage.loadFile(pdb).then(function(nucl) {
        window.ref_str_mol = nucl;
        window.ref_str_repr_prot = nucl.addRepresentation('spacefill', {
          "sele": 'protein',
          "color": '#29d6d9',
          "aspectRatio": 2,
          'radiusScale': 4.1,
          'radiusType': 'sstruc',
          "aspectRatio": 2,
          "radiusSegments": 1,
          "capped": 0

        });
        window.ref_str_repr_nucl = nucl.addRepresentation('spacefill', {
          "sele": 'nucleic',
          "color": '#29d6d9',
          "aspectRatio": 2,
          "radius": 1.51,
          "radiusSegments": 1,
          "capped": 0
        });
        window.ref_str_repr_base = nucl.addRepresentation('base', {
          "sele": 'nucleic',
          "color": '#29d6d9'
        });
      })
    }



    var margin = {
        top: 10,
        right: 30,
        bottom: 40,
        left: 60
      },
      width = 800 - margin.left - margin.right,
      height = 200 - margin.top - margin.bottom;

    // append the svg object to the body of the page
    var svg = d3.select("#my_dataviz")
      .append("svg")
      .attr("width", width + margin.left + margin.right)
      .attr("height", height + margin.top + margin.bottom)
      .append("g")
      .attr("transform",
        "translate(" + margin.left + "," + margin.top + ")");
    const tooltipLine = svg.append('line');
    var x = d3.scaleLinear();
    var y = d3.scaleLinear();

  })



</script>
    <br>



    <div id="viewport0" style="height:500px; border: thin solid black"></div>
    <div class="slidecontainer">
      <button type="submit" class="btn" name="play_button" data-toggle="button" id='play' onclick='window.traj.player.play();'>Play</button>
      <button type="submit" class="btn" name="play_button" data-toggle="button" id='pause' onclick='window.traj.player.pause();'>Pause</button>
      <input type="range" min="0" max="100" value="0" class="slider" id="myRange">
      <p>Frame: <span id="frame_counter"></span></p>

    </div>

  </body>
</html>
