# Separation, Cohesion and Alignment
#### 
I have developed a simulation of a group seagulls flying above the ocean. The movement of each individual bird in terms of its direction, velocity and distance from other seagulls is affected by the position and motion of the surrounding birds, in accordance with the principles of saperation, cohesion and alignment. Additionally, the simulation provides the controls for adjusting the paremeters of the model to customize the behavior of the virtual flock. 

# Youtube Link
####
https://youtu.be/pD3m2YlsPzA

# Code

```
import { setRootOrigin, draw, initStage } from './libs/displayUtils';
import { rng, cg } from './libs/cgUtils';
import { dialog } from './libs/dialogUtils';
import Point = CG.Base.geom.Point;
import Size = CG.Base.geom.Size;
import trans = CG.Base.translation.translate;

/* initialize */
let stageSize = new Size(960, 640);
cg.system.showFps();
dialog.init();
initStage(stageSize.width, stageSize.height, { transparent: true });
cg.pixi.stageResolutionPolicy = CG.Base.pixis.Pixi.RESOLUTION_POLICY.NO_BORDER;
setRootOrigin(0, 0);

/* have fun to play around */
let config = {
    birdsTotal: 100,
    birdVision: 30,
    maxSpeed: 3,
    accRate: 0.2,
    alignmentFactor: 1,
    cohesionFactor: 1,
    seperationFactor: 1,
};

class Bird {
    position = new Point(rng.nextBetween(0, stageSize.width), rng.nextBetween(0, stageSize.height));
    velocity = Point.polar(rng.nextBetween(0, config.maxSpeed), rng.nextBetween(0, Math.PI * 2));
    acceleration = new Point();
    force = new Point(0, 0);

    sprite = draw.image('seagullsAboveTheSea.seaGull', {
        scale: 0.15,
    })

    update(): void {
        this.velocity.limit(config.maxSpeed);

        this.position.x += this.velocity.x;
        this.position.y += this.velocity.y;
        this.velocity.x += this.acceleration.x * config.accRate;
        this.velocity.y += this.acceleration.y * config.accRate;

        if (this.position.x < 0) {
            this.position.x += stageSize.width;
        } else if (this.position.x > stageSize.width) {
            this.position.x -= stageSize.width;
        }

        if (this.position.y < 0) {
            this.position.y += stageSize.height;
        } else if (this.position.y > stageSize.height) {
            this.position.y -= stageSize.height;
        }

        //this.velocity.x += this.force.x * config.forceToAccRate;
        //this.velocity.y += this.force.y * config.forceToAccRate;

        this.sprite.x = this.position.x;
        this.sprite.y = this.position.y;
        this.sprite.rotation = this.velocity.angle;

    }



    generateForce(birds: Bird[]): void {
        this.force.x = 0;
        this.force.y = 0;

        let alignmentTotal = new Point();
        let cohesionTotal = new Point();
        let seperationTotal = new Point();
        let total = 0;

        for (let bird of birds) {
            
            let vector = this.getVectorToBird(bird);
            let distance = vector.length;

            if (distance < config.birdVision && bird != this) {
                total++;

                alignmentTotal.x += bird.velocity.x;
                alignmentTotal.y += bird.velocity.y;
                cohesionTotal.x += vector.x;
                cohesionTotal.y += vector.y;
                seperationTotal.x += -vector.x / distance / distance * config.birdVision;
                seperationTotal.y += -vector.y / distance / distance * config.birdVision;
            }
        }
        if (total != 0) {
            this.force.x += alignmentTotal.x / total * config.alignmentFactor;
            this.force.y += alignmentTotal.y / total * config.alignmentFactor;
            this.force.x += cohesionTotal.x / total * config.cohesionFactor;
            this.force.y += cohesionTotal.y / total * config.cohesionFactor;
            this.force.x += seperationTotal.x * config.seperationFactor;
            this.force.y += seperationTotal.y * config.seperationFactor;

            this.acceleration.x = this.force.x;
            this.acceleration.y = this.force.y;
            } else {
            this.acceleration.x = 0;
            this.acceleration.y = 0;

        }

    }

    getVectorToBird(bird: Bird): Point {
        let vector = bird.position.sub(this.position);
        if (vector.x > stageSize.width / 2) {
            vector.x -= stageSize.width;
        } else if (vector.x < -stageSize.width / 2) {
            vector.x += stageSize.width;
        }
        if (vector.y > stageSize.height / 2) {
            vector.y -= stageSize.height;
        } else if (vector.y < -stageSize.height / 2) {
            vector.y += stageSize.height;
        }
        return vector;
    }


}

let birds: Bird[] = [];
for (let i = 0; i < config.birdsTotal; i++) {
    birds.push(new Bird());
}

function updateAll(): void {
    for (let bird of birds) {
        bird.generateForce(birds);
        bird.update();
    }
}
cg.addUpdateFunction(updateAll);

draw.backgroundVideo('https://github.com/haskasu/haskatube-assets/blob/main/video/ocean-loop.mp4?raw=true', { maskColor: '#0236' });

dialog.createGUI()
    .then(gui => {
        gui.add(config, 'birdVision', 0, 300);
        gui.add(config, 'maxSpeed', 0, 10, 0.1);
        gui.add(config, 'alignmentFactor', 0, 2, 0.01);
        gui.add(config, 'cohesionFactor', 0, 2, 0.01);
        gui.add(config, 'seperationFactor', 0, 2, 0.01);
    })
    
    ```
