<template>
    <div>
        <iv-visualisation :title="pageName" :vue_config="vue_config" :page_number="3">
            <div id="spin" class="center" style="padding-top: 50px;">
                <div id="animation">

                </div>
            </div>

            <template #hotspots>
                <iv-pane position="left" format="full">
                    <iv-sidebar-content :showPagination="true">
                        <iv-sidebar-section title="Theory" icon="book-open">
                            The Centrifugal Force (given by <iv-equation-box :stylise="false" class="in-line-eqn" equation="-m\boldsymbol{\omega} \times (\boldsymbol{\omega} \times \mathbf{r})" />) is another of the inertial forces. This force <span style="color: red;">always points out away from the Earth</span> and so acts as a correction to gravity. <br><br>

                            It does not cause any deviations in the plane of the Earth's surface, but is accounted for by a slightly changed value of g which changes the shape of the Earth to an oblate spheroid (squashed orange). <br><br>
                            
                            At our rotation speed, there is a difference a difference in diameter of <iv-equation-box :stylise="false" class="in-line-eqn" equation="22\,km" /> between the poles and the equator. Use this animation to visualise how a slightly higher <iv-equation-box :stylise="false" class="in-line-eqn" equation="\boldsymbol{\omega}" /> would change the Earth.
                        </iv-sidebar-section>

                        <iv-sidebar-section title="Instructions" theme="Gold">
                            <strong>Slider: </strong>This controls <iv-equation-box :stylise="false" class="in-line-eqn" equation="\boldsymbol{\omega}" /> which ranges between 0 and 0.8. <br>
                            <strong>Show Original: </strong>Hover your mouse over this button to show the sphere that would be there if there was no rotation.
                        </iv-sidebar-section>
                    </iv-sidebar-content>
                </iv-pane>

                <iv-fixed-hotspot position="bottom" style="height: 25vh;">
                    <div class="center">
                        <div>
                            <iv-button @click="playing=!playing">Play</iv-button>   
                            <iv-button @mouseover="show=!show" @mouseleave="show=!show">Show Original</iv-button>
                        </div>
                        <iv-slider name="Control" :min="0" :max="0.8" :step="0.025"/>

                        
                    </div>
                    
                </iv-fixed-hotspot>
            </template>        
        </iv-visualisation>
    </div>
</template>
<script>
import vue_config from '../../vue.config.js';
import P5 from 'p5';
import $ from 'jquery';

export default {
    name:"centrifugal",
    data(){
        return {
            pageName:"Centrifugal Force",
            vue_config,
            image: require('@/assets/Earth.png'),
            playing: true,
            show: false,
        }
    },
    mounted() {
        /* eslint-disable no-unused-vars */
        let vm = this;
        let slider;
        let playing = this.playing;
        let omega = 0;
        let buttonIncrease, buttonDecrease, img;
        const radius = 160;
        let width = 450, height = 450; //Set's width to be equal to the div with id spin in the HTML page, ensures it has full width

        const spinview = p5 => {
            p5.preload = () => {
                img = p5.loadImage(this.image);
            }

            p5.setup = () => {
                let canvas = p5.createCanvas(width,height, "webgl");
                canvas.parent("spin");
                p5.background(25);

                slider = p5.createSlider(0, 0.8, 0.025, 0);
                slider.position(25, 0)
                slider.parent("animation")
                slider.style('width', '150px');
                let sliderLabel = p5.createP("Slider to control ω ")
                sliderLabel.parent("animation")
                sliderLabel.position(25,-25)
                sliderLabel.class("sliderText")

                function windowResized() {
                    p5.resizeCanvas($("#spin").width(), 0.6*p5.windowHeight);
                }
            }

            p5.draw = () => {
                if (this.playing) {
                    omega = slider.value()
                    let flatteningFactor = 1 - omega * omega;
                    // Mathematical expression for two semi-major axes of the oblate spheroid
                    let a = Math.sqrt(radius * radius / flatteningFactor);
                    p5.background(25);

                    p5.rotateY((2*omega)*p5.millis()/1000);
                    // Plots the original sphere so user can compare the flattened Earth to a non-rotating Earth in real time
                    if (vm.show) {
                        p5.noFill()
                        p5.stroke(225);
                        p5.sphere(radius)
                    } else {
                        p5.noStroke();
                    }
                    p5.fill(173, 25, 96);
                    p5.texture(img);

                    // Allows user's cursor to direct the light
                    let locX = p5.mouseX - height/2;
                    let locY = p5.mouseY - width/2;
                    p5.pointLight(255, 255, 255, 145, -155, 500)
                    p5.pointLight(255, 255, 255, locX, locY, 500)
                    p5.pointLight(255, 255, 255, locX, locY, 500)
                    p5.pointLight(255, 255, 255, locX, locY, 500)
                    p5.pointLight(255, 255, 255, locX, locY, 500)

                    p5.ellipsoid(a, radius*flatteningFactor, a, 24); // was 128
                }
            }
        }   

        const p5inst = new P5(spinview, 'spin');
    },
}
</script>
<style>
.center{
    display:flex;
    flex-direction: column;
    align-items: center;
}
.in-line-eqn {
    margin-top: -30px;
    margin-bottom: -30px;
}
</style>