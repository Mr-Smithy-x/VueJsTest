<template>
    <div class="hello">
        <div class="container">

            <h2 class="header">{{ name }}</h2>
            <div class="holder">
                <form @submit.prevent="addSkill">
                    <input type="text" placeholder="Enter a skill you have..." v-model="skill" v-validate="'min:5'"
                           name="skill">
                    <transition name="alert-in" enter-active-class="animated flipInX"
                                leave-active-class="animated flipOutX">
                        <p class="alert" v-if="errors.has('skill')">{{errors.first('skill')}}</p>
                    </transition>
                </form>
                <ul>
                    <transition-group name="list" enter-active-class="animated bounceInUp"
                                      leave-active-class="animated bounceOutDown">
                        <li v-for="(data, index) in skills" :key="index">
                            {{data.skill}}
                            <i class="fa fa-minus-circle" v-on:click="remove(index)"></i>
                        </li>
                    </transition-group>
                </ul>
            </div>

        </div>
    </div>
</template>

<script>
    export default {
        name: 'Skills',
        data() {
            return {
                name: 'Add 5 Skills',
                btnState: true,
                skills: [
                    {"skill": "Vue.js"},
                    {"skill": "Laravel"}
                ],
                skill: '',
                alertObject: {
                    alert: true
                }
            }
        },
        methods: {
            addSkill() {
                this.$validator.validateAll().then((result) => {
                    if (result && this.skills.length < 5) {

                        this.skills.push({skill: this.skill})
                        this.skill = '';
                    } else {
                        // eslint-disable-next-line no-console
                        console.log("Not Valid")
                    }
                })
            },
            remove(id) {
                this.skills.splice(id, 1);
            }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style src="./skill.css" scoped>

</style>
