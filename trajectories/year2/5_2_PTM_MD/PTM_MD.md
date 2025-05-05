### Траектории ПТМ в составе трипептидов  
[Назад](http://intbio.github.io/grant_2023_RNFmoluch/)

<html lang="en">
  <head>
    <meta charset="utf-8">
    <style>
      .trajectory-container {
        display: flex;
        flex-direction: column;
        align-items: center;
        margin-bottom: 40px;
      }
      .color-legend {
        text-align: center;
        margin-bottom: 10px;
      }
      .viewport {
        width: 500px;
        height: 500px;
        border: thin solid black;
      }
    </style>
  </head>

    <body>
    <div class="container-lg px-3 my-5 markdown-body">
      
      <h1 style="text-align: center;"><a href="http://intbio.org/grant_2023_RNFmoluch/">grant_2023_RNFmoluch</a></h1>
      

      <h3 id="траектории-птм-в-составе-трипептидов" style="text-align: center;">Траектории ПТМ в составе трипептидов</h3>
      <p style="text-align: center;"><a href="http://intbio.github.io/grant_2023_RNFmoluch/">Назад</a></p>

      <html lang="en">
      <head>
        <meta charset="utf-8" />
        <script src="https://unpkg.com/ngl@2.0.0-dev.35/dist/ngl.js"></script>
      </head>
      <body>
        <script>
          document.addEventListener("DOMContentLoaded", function() {
            // Define all trajectories
            const trajectories = [
              {name: 'Lysine_1M', resName: 'K1M', displayName: 'Lysine 1M'},
              {name: 'Lysine_2M', resName: 'K2M', displayName: 'Lysine 2M'},
              {name: 'Lysine_3M', resName: 'K3M', displayName: 'Lysine 3M'},
              {name: 'Lysine_1M_ACC', resName: 'KMA', displayName: 'Lysine 1M ACC'},
              {name: 'Lysine_Acetyl', resName: 'KAC', displayName: 'Lysine Acetyl'},
              {name: 'Lysine_Butyryl', resName: 'KBU', displayName: 'Lysine Butyryl'},
              {name: 'Lysine_Crotonyl', resName: 'KCR', displayName: 'Lysine Crotonyl'},
              {name: 'Lysine_Formyl', resName: 'KFO', displayName: 'Lysine Formyl'},
              {name: 'Lysine_Malonyl', resName: 'KML', displayName: 'Lysine Malonyl'},
              {name: 'Lysine_Propionyl', resName: 'KPR', displayName: 'Lysine Propionyl'}
            ];
            
            // Create viewports for each trajectory
            trajectories.forEach((traj, index) => {
              // Create container for each trajectory
              const container = document.createElement('div');
              container.className = 'trajectory-container';
              
              // Add color legend
              const legend = document.createElement('div');
              legend.className = 'color-legend';
              legend.innerHTML = `
                <p style="color:#FCDDF2;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">GLY</p>
                <p style="color:#808080;font-size:22px;font-family:verdana;font-weight: bold;text-shadow: -1px 0 black, 0 1px black, 1px 0 black, 0 -1px black;display: inline">${traj.displayName}</p>
              `;
              container.appendChild(legend);
              
              // Add title
              // const title = document.createElement('h3');
              // title.style.textAlign = 'center';
              // title.textContent = traj.name;
              // container.appendChild(title);
              
              // Add viewport
              const viewport = document.createElement('div');
              viewport.className = 'viewport';
              viewport.id = `viewport_${index}`;
              container.appendChild(viewport);
              
              document.body.appendChild(container);
              
              // Create stage and load files
              var stage = new NGL.Stage(`viewport_${index}`, { 
                backgroundColor: "#FFFFFF" 
              });
              
              // Load PDB structure
              stage.loadFile(`${traj.name}.pdb`).then(function(comp) {
                // Modified residue representation
                comp.addRepresentation('hyperball', {
                  "sele": traj.resName,
                  "color": "element",
                  "radius": 1.5
                });
                comp.addRepresentation('licorice', {
                  "sele": traj.resName,
                  "color": "element",
                  "radius": 0.20
                });
                
                // Glycine representation
                comp.addRepresentation('ball+stick', {
                  "sele": "GLY",
                  "color": "pink",
                  "radius": 0.17,
                });
                comp.addRepresentation('hyperball', {
                  "sele": "GLY",
                  "color": "pink",
                  "radius": 1.6
                });
                
                // Backbone representation
                comp.addRepresentation('licorice', {
                  "sele": "backbone",
                  "color": "element",
                  "radius": 0.169
                });
                
                // General protein coloring
                comp.addRepresentation('hyperball', {
                  "sele": "protein",
                  "color": "element",
                  "radius": 0.8
                });
                
                // Load trajectory
                NGL.autoLoad(`${traj.name}.xtc`).then(function(frames) {
                  comp.addTrajectory(frames);
                  var trajectory = comp.trajList[0].trajectory;
                  
                  // Create player with slower bounce mode
                  var player = new NGL.TrajectoryPlayer(trajectory, {
                    start: 0,
                    timeout: 150,
                    mode: "once",
                    interpolateType: "spline",
                    step: 1,
                    interpolateStep: 20,
                    direction: "bounce"
                  });
                  
                  // Start playing automatically
                  player.play();
                });
                
                // Auto view to fit the molecule
                comp.autoView();
              });
            });
          });
        </script>
      </body>
      </html>
      
    </div>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/anchor-js/4.1.0/anchor.min.js" integrity="sha256-lZaRhKri35AyJSypXXs4o6OPFTbTmUoltBbDCbdzegg=" crossorigin="anonymous"></script>
    <script>anchors.add();</script>
  </body>
</html>
