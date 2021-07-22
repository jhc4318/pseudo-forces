<template>
    <div>
        <iv-visualisation :title="pageName" :vue_config="vue_config" :page_number="1">
            <div class="grid-container" style="padding-top: 20px;">
                <div class="grid-item1 center">
                    <iv-button @click="show1=!show1" style="height: 40px;">Fixed in rotating frame</iv-button>
                    <div class="center" v-if="show1">
                        <p><i>Vector has no angular speed in the rotating frame.</i></p>
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="\mathbf{V_{Rotating}} = 0" />
                        <p><i>Using the standard equation <iv-equation-box class="in-line-eqn" :stylise="false" equation="v = \omega r" /></i></p>
                        <iv-equation-box class="equation" :stylise="true" equation="V_{Inertial} = \rho \omega = r \sin \theta * \omega = \omega r \sin \theta" /><br>
                        <iv-equation-box class="equation" :stylise="true" equation="\mathbf{V_{Inertial}} = \boldsymbol{\omega} \times \mathbf{r}" />
                        <div style="width: 100%;">
                            <SvgDiagram style="width: 100%"/>
                        </div>  
                    </div>
                    
                </div>
                <div class="grid-item2 center">
                    <iv-button @click="show2=!show2" style="height: 40px;">Moving in rotating frame</iv-button>
                    <div v-if="show2" class="center">
                        <br>
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="\mathbf{V_{Rotating}} \neq 0" />
                        <p><i>This means there are now two contributions to the velocity in the inertial frame: one from the vector rotating in the frame and one from the frame itself rotating relative to the inertial frame.</i></p>
                        <iv-equation-box :stylise="true" equation="\mathbf{V_{I}} = \mathbf{V_{R}} + \boldsymbol{\omega} \times \mathbf{r}" />
                        <p><i>where I and R refer to the inertial and rotating frames respectively.</i></p>
                        <p>This is a general result and (written in terms of derivatives) can relate any vector in the inertial frame to one in the rotating frame:</p>
                        <iv-equation-box :stylise="true" equation="\frac{d\boldsymbol{r}}{dt}|_{I} = \frac{d\boldsymbol{r}}{dt}|_{R} + \boldsymbol{\omega} \times \mathbf{r}" />
                        <p>which reduces to formula for a fixed point in rotating frame if <iv-equation-box class="in-line-eqn" :stylise="false" equation="\frac{d\boldsymbol{r}}{dt}|_{R} = 0." /></p>
                    </div>
                </div>
                <div class="grid-item3 center" >
                    <iv-button @click="show3=!show3" style="height: 40px;">Accelerating in rotating frame</iv-button>
                    <div v-if="show3" class="center">
                        <br>
                        <iv-equation-box :stylise="true" equation="\mathbf{a|_{I}} = \frac{d\mathbf{V_{I}}}{dt}|_{I} = \frac{d}{dt} (\mathbf{V_R} + \boldsymbol{\omega} \times \mathbf{r})|_{I}" />
                        <p><i>using the expression relating speeds between the frames.</i></p>
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="= \color{blue}{\frac{d}{dt}(\boldsymbol{V_{R}})|_{I}} \color{black}+ \color{red}{\frac{d}{dt}(\boldsymbol{\omega} \times \mathbf{r})|_{I}}" />
                        <p>Using the formula given previously relating derivatives in the rotating and inertial frames:</p>
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="= \color{blue}{\frac{d}{dt}(\boldsymbol{V_{R}})|_{R} + \boldsymbol{\omega} \times \mathbf{V_{R}}} \color{black}+ \color{red}{\boldsymbol{\omega} \times \frac{d\boldsymbol{r}}{dt}|_{I} + \frac{d \boldsymbol{\omega}}{dt}|_{I} \times \mathbf{r}}" />
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="= \color{blue}{\mathbf{a_{R}} + \boldsymbol{\omega} \times \mathbf{V_{R}}} \color{black}+ \color{red}{\boldsymbol{\omega} \times \mathbf{V_{I}} + \boldsymbol{\dot{\omega}} \times \mathbf{r}}" />
                        <p>Once again using the formula relating the velocities, we can substitute <iv-equation-box class="in-line-eqn" :stylise="false" equation="\mathbf{V_{I}}" /> for <iv-equation-box class="in-line-eqn" :stylise="false" equation="\mathbf{V_{R}} + \boldsymbol{\omega} \times \boldsymbol{r}" /></p>
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="= \color{blue}{\mathbf{a_{R}} + \boldsymbol{\omega} \times \mathbf{V_{R}}} \color{black}+ \color{red}{\boldsymbol{\omega} \times (\mathbf{V_{R}} + \boldsymbol{\omega} \times \mathbf{r}) + \boldsymbol{\dot{\omega}} \times \mathbf{r}}" />
                        <iv-equation-box class="in-line-eqn" :stylise="false" equation="= \mathbf{a_{R}} + 2\boldsymbol{\omega} \times \mathbf{V_{R}} + \boldsymbol{\omega} \times (\boldsymbol{\omega} \times \mathbf{r}) + \boldsymbol{\dot{\omega}} \times \mathbf{r}" />
                        <p><i>where the brackets are necessary otherwise <iv-equation-box class="in-line-eqn" :stylise="false" equation="\boldsymbol{\omega} \times \boldsymbol{\omega} = 0" /></i></p>
                        <p>Using <iv-equation-box class="in-line-eqn" :stylise="false" equation="\mathbf{F} = m \mathbf{a}" /> and the relation above, we find that the force in the rotating frame is the sum of the force in the inertial frame and 3 other <i>inertial forces.</i></p>
                        <iv-equation-box :stylise="true" equation="\mathbf{a|_{I}} = \frac{d\mathbf{V_{I}}}{dt}|_{I} = \frac{d}{dt} (\mathbf{V_R} + \boldsymbol{\omega} \times \mathbf{r})|_{I}" /><br>
                        <iv-equation-box :stylise="true" equation="\mathbf{F_{R}} = \mathbf{F_{I}} \color{green}-2m\boldsymbol{\omega \times \mathbf{V_{R}}} \\ \color{blue}-m\boldsymbol{\omega \times (\boldsymbol{\omega} \times \mathbf{r})} \color{red}-m\boldsymbol{\dot{\omega}} \times \mathbf{r}" />
                        <p>and these are the forces we give names to, being labelled as:</p>
                        <iv-equation-box :stylise="true" equation="\mathbf{F_{Rotating}} = {\mathbf{F_{Inertial}}} + \color{green}{\mathbf{F_{Coriolis}}} \\ + \color{blue}{\mathbf{F_{Centrifugal}}} + \color{red}{\mathbf{F_{Euler}}}" />
                    </div>
                    
                </div>
            </div>
        </iv-visualisation>
    </div>
</template>
<script>
import vue_config from '../../vue.config.js'
import SvgDiagram from './svgDiagram.vue'

export default {
    name:"derivation",
    components: {
        SvgDiagram,
    },
    data() {
        return {
            pageName:"Mathematical Derivation of Inertial Forces",
            vue_config,
            show1: false,
            show2: false,
            show3: false,
        }
    }
}
</script>
<style>
.center {
    display:flex;
    flex-direction: column;
    align-items: center;
    /* margin-top: 50px; */
}
.grid-container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 30px;
}
.grid-item1 {
    grid-column: 1 / 2;
}
.grid-item2 {
    grid-column: 2 / 3;
}
.grid-item3 {
    grid-column: 3 / 4;
}
p {
    text-align: center;
}
.in-line-eqn {
    margin-top: -20px;
    margin-bottom: -20px;
}
</style>