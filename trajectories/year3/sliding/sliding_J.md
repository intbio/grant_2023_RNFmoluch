### SHL +2, верхняя цепь, повторность 1 - траектория молекулярной динамики нуклеосомы с управляемым формированием торсионного дефекта (PDB ID 3LZ0)
[Назад](http://intbio.github.io/grant_2023_RNFmoluch/year3/)

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
  var pdb="sliding_J.pdb"
  var xtc="sliding_J.xtc"
  var trjstep = 2;
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

      

      window.arg_lys_selection = nucl.addRepresentation('hyperball', {
        "sele": "(ARG or LYS) and not _H",
        color: hyper_scheme,
        radius: 3.5
      });
      window.arg_lys_selection.setVisibility(false);
      
      window.dna_latch_selection = nucl.addRepresentation('hyperball', {
        "sele": "39-49 and (:A or :E) and not _H",
        color: residues,
        radius: 3.5
      });
      window.dna_latch_selection.setVisibility(false);
      
      //H3 39-49 - назвать H3 39-49 DNA latch

        window.replaced_AA_selection = nucl.addRepresentation('hyperball', {
        "sele": "(10 or 16 or 40 or 99 or 123 or 124 or 126 or 128) and (:C or :G) and not _H",
        color: hyper_scheme,
        radius: 3.5
      });
      window.replaced_AA_selection.setVisibility(false);


      nucl.addRepresentation('cartoon', {
        "sele": ":A :E",
        "color": 0x020AED,
        "aspectRatio": aspectRatio,
        'radiusScale': radiusScale,
        'radiusType': 'sstruc',
        "capped": true,
        'subdiv': 10,
        'diffuseInterior': false,
        'useInteriorColor': false
      });
      nucl.addRepresentation('cartoon', {
        "sele": ":B :F",
        "color": "green",
        "aspectRatio": aspectRatio,
        'radiusScale': radiusScale,
        'radiusType': 'sstruc',
        "capped": true,
        'subdiv': 10,
        'diffuseInterior': false,
        'useInteriorColor': false
      });
      nucl.addRepresentation('cartoon', {
        "sele": ":C :G",
        "color": 0xE0F705,
        "aspectRatio": aspectRatio,
        'radiusScale': radiusScale,
        'radiusType': 'sstruc',
        "capped": true,
        'subdiv': 10,
        'diffuseInterior': false,
        'useInteriorColor': false
      });
      nucl.addRepresentation('cartoon', {
        "sele": ":D :H",
        "color": 0xCE0000,
        "aspectRatio": aspectRatio,
        'radiusScale': radiusScale,
        'radiusType': 'sstruc',
        "capped": true,
        'subdiv': 10,
        'diffuseInterior': false,
        'useInteriorColor': false
      });
      nucl.addRepresentation('spacefill', {
        "sele": ":I and -72 and .C1'",
        "color": "steelblue",
        "radius":5,
        'diffuseInterior': false,
        'useInteriorColor': false
      });
      nucl.addRepresentation('spacefill', {
        "sele": ":J and -72 and .C1'",
        "color": "orange",
        "radius":5,
        'diffuseInterior': false,
        'useInteriorColor': false
      });
      window.nucl_cartoon_J = nucl.addRepresentation('cartoon', {
        "sele": "nucleic",
        "color": 'grey',
        "aspectRatio": aspectRatio,
        "radius": radius,
        "radiusSegments": 1,
        "capped": 0
      });
      window.nucl_cartoon_I = nucl.addRepresentation('cartoon', {
        "sele": ":J and -17-61",
        "color": 'cyan',
        "aspectRatio": aspectRatio,
        "radius": radius,
        "radiusSegments": 1.1,
        "capped": 0
      });
	  window.nucl_cartoon_mov = nucl.addRepresentation('cartoon', {
        "sele": "nucleic and (:J and -60--25)",
        "color": 'magenta',
        "aspectRatio": aspectRatio,
        "radius": radius,
        "radiusSegments": 1.1,
        "capped": 0
      });
      window.nucl_base = nucl.addRepresentation('base', {
        "sele": "nucleic",
        "color": 'grey'
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
          $("#frame_counter")[0].innerHTML = (fnum * trjstep).toFixed(2);
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


    $('input[type=checkbox][name=ref_str_check]').on('change', toggle_reference_structure);
    $('input[type=checkbox][name=arg_lys_check]').on('change', toggle_lys_arg_visibility);
    $('input[type=checkbox][name=latch_check]').on('change', toggle_latch_visibility);
    $('input[type=checkbox][name=highlight_DA_check]').on('change', toggle_DA_highlight);
    $('input[type=checkbox][name=ortho_check]').on('change', toggle_orthographic);
    $('input[type=checkbox][name=axes_check]').on('change', toggle_axes_visibility);
    $('input[type=checkbox][name=replaced_AA_check]').on('change', toggle_replaced_AA_visibility);
    


    function load_reference_structure() {
      window.ref_str_mol = window.stage.loadFile(pdb).then(function(nucl) {
        window.ref_str_mol = nucl;
        window.ref_str_repr_prot = nucl.addRepresentation('cartoon', {
          "sele": 'protein',
          "color": '#29d6d9',
          "aspectRatio": 2,
          'radiusScale': 4.1,
          'radiusType': 'sstruc',
          "aspectRatio": 2,
          "radiusSegments": 1,
          "capped": 0

        });
        window.ref_str_repr_nucl = nucl.addRepresentation('cartoon', {
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

    function toggle_reference_structure() {
      var state = $(this).is(":checked");
      if (typeof window.ref_str_repr_prot == "undefined") {
        load_reference_structure();

      } else {
        window.ref_str_repr_prot.setVisibility(state);
        window.ref_str_repr_nucl.setVisibility(state);
        window.ref_str_repr_base.setVisibility(state)
      }


    }

    function toggle_DA_highlight() {
      var state = $(this).is(":checked");
      if (state === false) {
        window.nucl_cartoon_J.setColor('grey')
        window.nucl_cartoon_I.setColor('cyan')
        window.nucl_cartoon_mov.setColor('magenta')
        window.nucl_base.setColor('grey')
      } else {
        var scheme = NGL.ColormakerRegistry.addSelectionScheme([
          ["pink", "DA"],
          ["grey", "*"]
        ], "DA");
        window.nucl_cartoon_J.setColor(scheme)
        window.nucl_cartoon_I.setColor(scheme)
        window.nucl_cartoon_mov.setColor(scheme)
        window.nucl_base.setColor(scheme)

      }
    }

    function toggle_lys_arg_visibility() {
      var state = $(this).is(":checked");
      window.arg_lys_selection.setVisibility(state);
    }
    function toggle_latch_visibility() {
      var state = $(this).is(":checked");
      window.dna_latch_selection.setVisibility(state);
    }

    function toggle_replaced_AA_visibility() {
      var state = $(this).is(":checked");
      window.replaced_AA_selection.setVisibility(state);
    }
    
    function toggle_axes_visibility() {
      var state = $(this).is(":checked");
      window.axes.setVisibility(state);
    }
    
    function toggle_orthographic() {
      var state = $(this).is(":checked");
      if ($(this).is(":checked")){
      	window.stage.setParameters({cameraType: "orthographic"})
        } else {
        window.stage.setParameters({cameraType: "perspective"})
        }
    }
  });
  
</script>
    <br>
    <p style="color:#020AED;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">H3</p>
    <p style="color:#009933;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">H4</p>
    <p style="color:#E0F705;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">H2A</p>
    <p style="color:#CE0000;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">H2B</p><br> 
    <p style="color:#808080;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">ДНК</p><br> 
	<p style="color:#FF00FF;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">ДНК, смещаемый фрагмент</p><br> 
	<p style="color:#00FFFF;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">ДНК, фрагмент для выравнивания</p>
    <br>
    <p><span style="color:steelblue;font-family:verdana;font-weight: bold">Синий</span> и <span style="color:orange;font-family:verdana;font-weight: bold">оранжевый</span> шары обозначают <span style="color:steelblue;font-family:verdana;font-weight: bold">проксимальный</span> и <span style="color:orange;font-family:verdana;font-weight: bold">дистальный</span> концы ДНК</p>
    <input class="form-check-input " type="checkbox" name="ref_str_check" value="" id="ref_str_check">
    <label class="form-check-label " for="ref_str_check">
      Show starting state
    </label>
    
    <input class="form-check-input " type="checkbox" name="ortho_check" value="" id="ortho_check" checked="true">
    <label class="form-check-label " for="ortho_check">
      Orthographic
    </label>
    
    <input class="form-check-input " type="checkbox" name="axes_check" value="" id="axes_check">
    <label class="form-check-label " for="ortho_check">
      Show axes
    </label>
    
    
    
    <input class="form-check-input " type="checkbox" name="arg_lys_check" value="" id="arg_lys_check">
    <label class="form-check-label " for="arg_lys_check">
      Show ARG LYS
    </label> 
    <!--
    <input class="form-check-input " type="checkbox" name="latch_check" value="" id="latch_check">
    <label class="form-check-label " for="latch_check">
      Show H3 39-49 DNA latch
    </label> -->

    <input class="form-check-input " type="checkbox" name="highlight_DA_check" value="" id="highlight_DA_check">
    <label class="form-check-label " for="highlight_DA_check">
      Highlight ADE
    </label>


    <div id="viewport0" style="height:500px; border: thin solid black"></div>
    <div class="slidecontainer">
      <button type="submit" class="btn" name="play_button" data-toggle="button" id='play' onclick='window.traj.player.play();'>Play</button>
      <button type="submit" class="btn" name="play_button" data-toggle="button" id='pause' onclick='window.traj.player.pause();'>Pause</button>
      <input type="range" min="0" max="100" value="0" class="slider" id="myRange">
      <p>Time: <span id="frame_counter"></span> ns</p>

    </div>
    <figure>
      <img src="relative_twist_sliding_J.png" alt="reltwist" style="width:100%">
    </figure>
        <figure>
      <img src="angles_sliding_J.png" alt="angles" style="width:88%">
    </figure>
  </body>
</html>
