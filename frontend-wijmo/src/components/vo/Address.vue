<template>
    <div style="margin: 0 -15px 0 -15px;">
        <v-card-title  v-if="editMode">
            {{label}}
        </v-card-title>
        <v-card-text v-if="value">
            <div v-if="editMode" style="margin-top:-20px;">
                <v-text-field label="Country" v-model="value.country" />
            </div>
            <div v-if="editMode" style="margin-top:-20px;">
                <v-text-field label="State" v-model="value.state" placeholder="OO도"/>
            </div>
            <div v-if="editMode" style="margin-top:-20px;">
                <v-text-field label="City" v-model="value.city"/>
            </div>
            <div v-if="editMode" style="margin-top:-20px;">
                <v-text-field label="Street" v-model="value.street"/>
            </div>
            <div v-if="editMode" style="margin-top:-20px;">
                <v-text-field label="Zipcode" v-model="value.zipcode" />
            </div>

            <div v-if="!editMode">
                <span style="font-family:sans-serif; font-weight:bold; font-size:15px"></span>
                <v-list-group
                    :value="inList"
                    @click="reSizeGrid"
                >
                    <template v-slot:activator>
                        <v-list-item-title>{{value.country }} / {{value.state }} {{value.city }}  {{value.street }} ({{value.zipcode }})</v-list-item-title>
                    </template>

                    <v-list-item>
                        <GmapMap
                            class="mx-auto"
                        :center="{lat:latitude, lng:longitude}"
                        :zoom="10"
                        map-type-id="terrain"
                        style="width: 200px; height: 250px"
                        >
                            <GmapMarker
                                :key="index"
                                v-for="(m, index) in markers"
                                :position="m.position"
                                :clickable="true"
                                :draggable="true"
                                @click="center=m.position"
                            />
                        </GmapMap>
                    </v-list-item>
                </v-list-group>
            </div>
        </v-card-text>
    </div>
</template>

<script>
    import Vue from 'vue'
    import * as VueGoogleMaps from 'vue2-google-maps'

    Vue.use(VueGoogleMaps, {
        load: {
            key: 'AIzaSyBupK0cTa2fQJA833My7W-BGLAYvQQEN1Q',
            libraries: 'places', // This is required if you use the Autocomplete plugin
            region: "KR",
        },
    })


    export default {
        name:"Address",
        props: {
            editMode: Boolean,
            value : Object,
            label : String,
            inList: Boolean
        },
        data: () => ({
            latitude: 37.5666805,
            longitude: 126.9784147,
            markers:[]
        }),
        created(){
            if(!this.value) {
                this.value = {
                    'street': '',
                    'city': '',
                    'state': '',
                    'country': '',
                    'zipcode': '',
                };
            }

            if(this.value.state.includes("경기도")){
                this.latitude = 37.567167;
                this.longitude = 127.190292;
            }else if(this.value.state.includes("강원도")){
                this.latitude = 37.555837;
                this.longitude = 128.209315;
            }else if(this.value.state.includes("충청남도")){
                this.latitude = 36.557229;
                this.longitude = 126.779757;
            }else if(this.value.state.includes("충청북도")){
                this.latitude = 36.628503;
                this.longitude = 127.929344;
            }else if(this.value.state.includes("경상북도")){
                this.latitude = 36.248647;
                this.longitude = 128.664734;
            }else if(this.value.state.includes("경상남도")){
                this.latitude = 35.259787;
                this.longitude = 128.664734;
            }else if(this.value.state.includes("전라북도")){
                this.latitude = 35.716705;
                this.longitude = 127.144185;
            }else if(this.value.state.includes("전라남도")){
                this.latitude = 34.819400;
                this.longitude = 126.893113;
            }else if(this.value.state.includes("제주")){
                this.latitude = 33.364805;
                this.longitude = 126.542671;
            }else if(this.value.state.includes("서울")){
                this.latitude = 37.487935;
                this.longitude = 126.857758;
            }else if(this.value.state.includes("인천")){
                this.latitude = 37.45639;
                this.longitude = 126.70528;
            }else if(this.value.state.includes("대전")){
                this.latitude = 36.35111;
                this.longitude = 127.38500;
            }else if(this.value.state.includes("대구")){
                this.latitude = 35.87222;
                this.longitude = 128.60250;
            }else if(this.value.state.includes("부산")){
                this.latitude = 35.17944;
                this.longitude = 129.07556;
            }else if(this.value.state.includes("광주")){
                this.latitude = 35.15972;
                this.longitude = 126.85306;
            }
        },
        watch: {
            value(newVal) {
                this.$emit('input', newVal);
            },
        },
    }
</script>

<style scoped>
</style>