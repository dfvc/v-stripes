<template>
    <div>
        <!-- <img :src="compSrc"> -->
    </div>
</template>


<script>
    export default {
        name: 'VStripes',

        data() {
            return {
                image: null,
                canvas: null,
                context: null,
            }
        },

        props: {
            baseSrc: {
                type: String,
                required: true,
            },
            stripeSize: {
                type: Number,
                default: 50,
            },
            stripeGap: {
                type: Number,
                default: 1,
            },
            stripeOffset: {
                type: Number,
                default: 50,
            },
            stripeOrientation: {
                type: String,
                default: 'landscape',
            }
        },

        computed: {
            compSrc() {
                return require(`@/assets/${this.baseSrc}`);
            },
        },

        mounted() {
            this.initData();
            this.registerOnLoadImg();
            this.loadImage();
        },

        methods: {
            initData() {
                this.image = new Image();
                this.canvas = document.createElement('canvas');
                this.context = this.canvas.getContext('2d');
            },

            registerOnLoadImg() {
                this.image.onload = () => {
                    this.setupCanvas();
                    this.drawStripes();
                }
            },

            setupCanvas() {
                if (this.stripeOrientation === 'landscape') {
                    this.canvas.width = this.image.width + this.stripeOffset;
                    this.canvas.height = this.image.height + this.stripeGap * (this.image.height / this.stripeSize);
                }
                else {
                    this.canvas.width = this.image.width + this.stripeGap * (this.image.width / this.stripeSize);
                    this.canvas.height = this.image.height + this.stripeOffset;
                }
                this.$el.appendChild(this.canvas);
            },

            loadImage() {
                this.image.src = this.compSrc;
            },

            drawStripes() {
                if (this.stripeOrientation === 'landscape') {
                    for (var i = 0; i < this.image.height / this.stripeSize; i ++) {
                        this.context.drawImage(
                            this.image, 
                            0, i * this.stripeSize, 
                            this.image.width, this.stripeSize, 
                            i % 2 === 0 ? this.stripeOffset : 0, i * this.stripeSize + i * this.stripeGap, 
                            this.image.width, this.stripeSize
                        );
                    }
                }
                else {
                    for (var i = 0; i < this.image.width / this.stripeSize; i ++) {
                        this.context.drawImage(
                            this.image, 
                            i * this.stripeSize, 0, 
                            this.stripeSize, this.image.height, 
                            i * this.stripeSize + i * this.stripeGap, i % 2 === 0 ? this.stripeOffset : 0,
                            this.stripeSize, this.image.height
                        );
                    }
                }
            },
        },
    }
</script>

