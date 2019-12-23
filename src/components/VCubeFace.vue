<template>
    <div 
        :style="faceStyles"
        class="cube__face"
    >
        {{ content }}
    </div>
</template>


<script>
    export default {
        name: 'VCubeFace',

        data() {
            return {
                faceStyles: {
                    'background-color': this.bgColor,
                    'border': `${this.showEdge ? this.edgeTickness : 0}px solid ${this.color}`,
                },
            }
        },

        props: {
            edgeSize: {
                type: Number,
                required: true,
            },
            edgeTickness: {
                type: Number,
                default: 1
            },
            showEdge: {
                type: Boolean,
                default: true,
            },
            bgColor: {
                type: String,
                default: '#fff',
            },
            color: {
                type: String,
                default: '#000',
            },
            content: {
                type: String,
                default: '',
            }
        },
    }
</script>


<style lang="scss" scoped>
    .cube {

        &__face {
            position: absolute;
            display: flex;
            justify-content: center;
            align-items: center;
            width: 100%;
            height: 100%;
        }
    }
</style>

