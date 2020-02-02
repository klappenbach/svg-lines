<template>

            <div style="height: 1800px" id="svgDiv">
            </div>
        
</template>

<script >
    import Matter from 'matter-js';
    import Two from 'two.js';
    
    export default {
        name: 'Chain',
        computed: {
            chainRope: function () {
                return this.rrope;
            }
        },
        data: function () {
            return {
                name: 'Chain',
                rrope: undefined
            }
        },
        mounted: function () {
            const elem = document.getElementById('chainDiv');
            var svgElem = document.getElementById('svgDiv');
            var params = { 
                height: '100%',
                width: '100%',
            };
            var two = new Two(params).appendTo(svgElem);
            //two.width = 800;
            //two.height = 800;
            
            
            var Engine = Matter.Engine,
                Render = Matter.Render,
                Runner = Matter.Runner,
                Body = Matter.Body,
                Composite = Matter.Composite,
                Composites = Matter.Composites,
                Constraint = Matter.Constraint,
                MouseConstraint = Matter.MouseConstraint,
                Mouse = Matter.Mouse,
                World = Matter.World,
                Bodies = Matter.Bodies,
                Events = Matter.Events;

            // create engine
            var engine = Engine.create(),
                world = engine.world;
            engine.world.gravity.y = 0;

            // create renderer
            


            // create runner
            var runner = Runner.create();
            Runner.run(runner, engine);

            // add bodies
            var group = Body.nextGroup(true);

            var ropeC = Composites.stack(0, 0, 24, 1, 10, 10, function(x, y) {
                return Bodies.rectangle(x - 20, y - 7, 50, 14, {
                    collisionFilter: { group: group },
                    chamfer: 0,
                    frictionAir: 0.5,
                    friction: 1,
                    frictionStatic: 100,
                    render: {
                        visible: true
                    }
                });
            });

            Composites.chain(ropeC, 0.3, 0, -0.3, 0, {
                stiffness: 1,
                length: 0,
                render: {
                    visible: true
                }
            });

            Composites.chain(ropeC, 0.3, 0.5, -0.3, 0.5, {
                stiffness: 0.99,
                damping: 1,
                length: 0,
                render: {
                    visible: false
                }
            });

            ///*
            Composite.add(ropeC, Constraint.create({
                bodyB: ropeC.bodies[0],
                pointB: { x: -20, y: 0 },
                pointA: { x: ropeC.bodies[0].position.x, y: ropeC.bodies[0].position.y },
                stiffness: 1
            }));
            //*/
            World.add(world, [
                ropeC,
                Bodies.rectangle(400, 600, 1200, 50.5, { isStatic: true })
            ]);

            // add mouse control
            var mouse = Mouse.create(svgElem),
                mouseConstraint = MouseConstraint.create(engine, {
                    mouse: mouse,
                    constraint: {
                        stiffness: 0.3,
                        angularStiffness: 1,
                        render: {
                            visible: false
                        }
                    }
                });
            //Mouse.setOffset(mouse, [0, 0]);

            // keep the mouse in sync with rendering
            //render.mouse = mouse;

            // fit the render viewport to the scene

            Engine.run(engine)

            World.add(world, mouseConstraint);
            //Render.run(render);
            // console.log(ropeC.bodies.constraint.stiffness);
            const vertices = ropeC.bodies.map(function(body){ return new Two.Anchor(body.position.x, body.position.y)});
            const chain = two.makeCurve(vertices, false, true);
            chain.linewidth = 10;
            chain.stroke = 'purple';
            
            chain.cap= 'round';
            chain.translation.x = 0;
            chain.scale = 1;
            chain.id = 'chain';
            console.log(chain.class);
            two.bind('update', function(frameCount) {
                const vertices = ropeC.bodies.map(function(body){ return new Two.Anchor(body.position.x, body.position.y)});
                chain.vertices = vertices;

            }).play();


            var vueObj = this;
            Events.on(engine, "afterUpdate", function () {
                vueObj.rrope = ropeC.bodies;
            });


        }
    };

    
</script>

<style scoped>

</style>
