<template>
    <div
        :style="cubeHolderStyles" 
        class="cube-holder"
    >    
        <div 
            :style="cubeStyles"
            class="cube"
        >
            <v-cube-face 
                v-for="(faceValue, faceKey) in faces"
                :key="faceKey"
                :edge-size="edgeSize" 
                :show-edge="faceValue.showEdge" 
                :bg-color="faceValue.bgColor" 
                :content="faceValue.content"
                :style="faceValue.styles"
                :class="`cube__face--${faceKey}`"
            />
        </div>
    </div>
</template>


<script>
    import VCubeFace from './VCubeFace.vue'

    export default {
        name: 'VCube',

        components: {
            VCubeFace
        },

        data() {
            return {
                allowedKeys: [37, 39, 38, 40],
                currXRotationDegrees: 0,
                currYRotationDegrees: 0,
                currZRotationDegrees: 0,
                currRotationCount: 0,
                isRotating: false,
                cubeHolderStyles: {
                    'width': `${this.edgeSize}px`,
                    'height': `${this.edgeSize}px`,
                    'perspective': `${this.edgeSize * 3}px`,
                },
                cubeStyles: {
                    'transform': `translateZ(-${this.edgeSize / 2}px)`,
                    'transition-duration': `${this.rotationDuration}ms`,
                    'transition-timing-function': 'cubic-bezier(0.075, 0.82, 0.165, 1'
                },
                faces: {
                    front: {
                        bgColor: 'rgba(255, 255, 255, 1)',
                        showEdge: this.showEdge,
                        content: '1',
                        styles: {
                            'transform': `rotateY(0deg) translateZ(${this.edgeSize / 2}px)`
                        },
                    },
                    right: {
                        bgColor: 'rgba(255, 0, 0, 1)',
                        showEdge: this.showEdge,
                        content: '2',
                        styles: {
                            'transform': `rotateY(90deg) translateZ(${this.edgeSize / 2}px)`
                        },
                    },
                    back: {
                        bgColor: 'rgba(0, 255, 0, 1)',
                        showEdge: this.showEdge,
                        content: '3',
                        styles: {
                            'transform': `rotateY(180deg) translateZ(${this.edgeSize / 2}px)`
                        },
                    },
                    left: {
                        bgColor: 'rgba(0, 0, 255, 1)',
                        showEdge: this.showEdge,
                        content: '4',
                        styles: {
                            'transform': `rotateY(-90deg) translateZ(${this.edgeSize / 2}px)`
                        },
                    },
                    top: {
                        bgColor: 'rgba(255, 255, 0, 1)',
                        showEdge: this.showEdge,
                        content: '5',
                        styles: {
                            'transform': `rotateX(90deg) translateZ(${this.edgeSize / 2}px)`
                        },
                    },
                    bottom: {
                        bgColor: 'rgba(0, 255, 255, 1)',
                        showEdge: this.showEdge,
                        content: '6',
                        styles: {
                            'transform': `rotateX(-90deg) translateZ(${this.edgeSize / 2}px)`
                        },
                    },
                },
            }
        },

        props: {
            edgeSize: {
                type: Number,
                default: 200,
            },
            showEdge: {
                type: Boolean,
                default: true,
            },
            rotationDuration: {
                type: Number,
                default: 500,
            },
            rotationStep: {
                type: Number,
                default: 90,
            },
            autoRotate: {
                type: Boolean,
                default: false,
            },
            autoRotateDelayFactor: {
                type: Number,
                default: 3,
            },
            maxRotations: {
                type: Number,
                default: -1,
            },
        },

        mounted() {
            if (this.autoRotate) {
                this.startAutoRotate();
            }
            else {
                window.addEventListener("keydown", this.onKeyDown);
            }
        },

        methods: {
            onKeyDown(event) {
                if (!this.isValidRotationKey(event.which) || this.isRotating) {
                    return;
                }

                event.preventDefault();
                this.rotateTo(event.which);
            },

            startAutoRotate() {
                let randDirection = this.allowedKeys[Math.floor(Math.random() * this.allowedKeys.length)];

                setTimeout(() => {
                    this.rotateTo(randDirection);
                    this.startAutoRotate();
                }, this.rotationDuration * this.autoRotateDelayFactor);
            },

            rotateTo(dirKeyCode) {
                if (!this.canRotate()) {
                    return;
                }
                
                this.updateRotationDegrees(dirKeyCode);
                
                this.cubeStyles = {
                    'transform': `translateZ(-${this.edgeSize / 2}px) rotateX(${this.currXRotationDegrees}deg) rotateY(${this.currYRotationDegrees}deg) rotateZ(${this.currZRotationDegrees}deg)`,
                    'transition-duration': `${this.rotationDuration}ms`,
                    'transition-timing-function': 'cubic-bezier(0.075, 0.82, 0.165, 1'
                };
                
                this.isRotating = true;
                setTimeout(() => {
                    this.isRotating = false;
                }, this.rotationDuration);

                this.currRotationCount ++;
            },

            updateRotationDegrees(dirKeyCode) {
                switch (dirKeyCode) {
                    case 39:  // Left
                        this.currYRotationDegrees += this.rotationStep;
                        break;
                    case 37:  // Right
                        this.currYRotationDegrees -= this.rotationStep;
                        break;
                    case 38:  // Up
                        this.currXRotationDegrees += this.rotationStep;
                        break;
                    case 40:  // Down
                        this.currXRotationDegrees -= this.rotationStep;
                        break;
                    default:
                        break;
                }
            },

            isValidRotationKey(keyCode) {
                return this.allowedKeys.includes(keyCode);
            },

            canRotate() {
                return this.maxRotations === -1 || this.currRotationCount < this.maxRotations;
            },
        },
    }
</script>


<style lang="scss" scoped>
    .cube {
        position: relative;
        width: 100%;
        height: 100%;
        transform-style: preserve-3d;
        transition-property: transform;
    }
</style>
