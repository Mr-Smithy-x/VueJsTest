<template>
    <div class="hello">
        <p>{{ name }}</p>
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

            <p v-if="skills.length >= 1">You have more than 1 skill</p>
            <p v-else>You have less than or equal to 1 skills</p>

        </div>
    </div>
</template>

<script>
    export default {
        name: 'Skills',
        data() {
            return {
                name: 'My Programming Skills',
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
                    if (result) {

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
