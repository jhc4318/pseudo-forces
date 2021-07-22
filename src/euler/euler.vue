<template>
    <div>
        <iv-visualisation :title="pageName" :vue_config="vue_config" :page_number="2">
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
            </template>
        </iv-visualisation>

        <div id="spin"></div>
    </div>
</template>
<script>
import vue_config from '../../vue.config.js';

export default {
    name:"euler",
    data(){
        return {
            pageName:"Centrifugal Force",
            vue_config
        }
    },
    mounted() {
        
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
html {
    overflow: hidden;
}
</style>