<template>
    <div class="simon">
        <div class="game">
            <div class="game-display" ref="gameDisplay" :style="{height: height+'px'}" >
                <div :class="green" @click="response(0)"></div>
                <div :class="red" @click="response(1)"></div>
                <div :class="blue" @click="response(2)"></div>
                <div :class="yellow" @click="response(3)"></div>
                <div class="center"></div>
            </div>
            <div class="text" :style="{height: height+'px'}">
                <div class="title">
                    Simon
                </div>
                <div class="count">
                    <div class="score">
                        Score : {{count}}
                    </div>
                </div>
            </div>
        </div>
        <div class="btn-game" @click="start" v-if="!end">
            Start the Game
        </div>
        <div class="btn-game" @click="resetGame" v-if="end">
            Reset Game
        </div>
    </div>
</template>

<script>
export default {
    name:'Simon',
    data(){
        return{
            color: [{
                g: 'green',
                r: 'red',
                y: 'yellow',
                b: 'blue'
            }],
            count: 0,
            end: false,
            tab: [],
            res: [],
            height: 0
        }
    },
    mounted(){
        this.setHeight()
    },
    computed:{
        green(){
            return 'quarter ' + this.color[0].g
        },
        red(){
            return 'quarter ' + this.color[0].r
        },
        blue(){
            return 'quarter ' + this.color[0].b
        },
        yellow(){
            return 'quarter ' + this.color[0].y
        }
    },
    created() {
        window.addEventListener("resize", this.setHeight);
    },
    destroyed() {
        window.removeEventListener("resize", this.setHeight);
    },
    methods:{
        start(){
            this.end = false;
            this.game();
        },
        game(){
                this.end = true;
                let inter = 0;

                let r = Math.floor(Math.random() * 4);
                this.tab[this.count] = r;

                this.count++;
                for(let i = 0; i < this.count; i++){
                    setTimeout(() => { 
                        if(this.tab[i] == 0){
                            this.color[0].g = this.color[0].g +' active'; 
                            setTimeout(() => { 
                                this.resetQuarter(this.color[0].g)
                            }, 500);
                        } else if(this.tab[i] == 1){
                            this.color[0].r = this.color[0].r +' active'; 
                            setTimeout(() => { 
                                this.resetQuarter(this.color[0].r)
                            }, 500);
                        } else if(this.tab[i] == 2){
                            this.color[0].b = this.color[0].b +' active'; 
                            setTimeout(() => { 
                                this.resetQuarter(this.color[0].b)
                            }, 500);
                        } else {
                            this.color[0].y = this.color[0].y +' active'; 
                            setTimeout(() => { 
                                this.resetQuarter(this.color[0].y)
                            }, 500);
                        }
                    }, inter);
                    inter += 1000;
                }
        },
        resetQuarter(quarter){
            this.color[0].g = quarter == this.color[0].g ? 'green' : this.color[0].g;
            this.color[0].r = quarter == this.color[0].r ? 'red' : this.color[0].r;
            this.color[0].b = quarter == this.color[0].b ? 'blue' : this.color[0].b;
            this.color[0].y = quarter == this.color[0].y ? 'yellow' : this.color[0].y;
        },
        response(num){
            this.res.push(num);

            if(this.res.length -1 != this.tab.length -1 ){
                if(this.res[this.res.length - 1] != this.tab[this.res.length - 1]){
                    this.resetGame();
                    this.end = false;
                }
            } else if(this.res[this.res.length -1] == this.tab[this.tab.length -1]){
                this.res = [];
                setTimeout(() => { 
                    this.game();
                }, 500);
            } else {
                this.resetGame();
                this.end = false;
            }
        },
        resetGame(){
            this.tab = [];
            this.count = 0;
            this.res = [];
            this.end = false;
        },
        setHeight(){
            this.height = this.$refs.gameDisplay.clientWidth;
        }
    }
}
</script>

<style lang="scss">
.simon{
    width: 750px;
    height: auto;

    background: var(--color-second);

    border-radius: 15px;
    box-shadow: 0px 15px 35px -5px rgba(50, 88, 130, 0.32);;

    z-index: 2;
    
    .game{
        display: flex;
    }

    .game-display{
        display: flex;
        flex: 1 0 45%;
        flex-wrap: wrap;
        
        position: relative;

        margin-top: 15px;
        margin-left: 15px;

        border-radius: 50%;
        border: solid 1px black;
        background: #000;

        .quarter{
            width: 50%;
            height: 50%;

            transition: all .3s ease-out;
        }

        .quarter:hover{
            cursor: pointer;
            opacity: 0.5;
        }

        .green{
            background:green;
            border-radius: 100% 0 0 0;
        }

        .red{
            background: red;
            border-radius: 0 100% 0 0;
        }

        .blue{
            background: blue;
            border-radius: 0 0 0 100%;
        }

        .yellow{
            background: yellow;
            border-radius: 0 0 100% 0;
        }

        .active{
            opacity: 0.5;
        }

        .center{
            width: 50%;
            height: 50%;
            background-color: #000;
            position: absolute;
            top: 25%;
            left: 25%;
            border-radius: 50%;
            border: 3px solid #000;
            z-index: 2;
        }

        .middle:hover{
            cursor: default;
        }
    }

    .btn-game{
        margin-left: auto;
        margin-right: auto;
        
        background: var(--color-third);
        border-radius: 5px;
        color: #fff;

        text-align: center;
        transition-duration: .3s;

        margin-bottom: 25px;
    }

    .btn-game:hover{
        cursor: pointer;
        transform: scale(1.1);
    }
    .text{
        position: relative;
        width: 25%;

        margin-top: 15px;

        .title{
            color: var(--text-second);
            font-weight: 600;
            text-transform: uppercase;
            letter-spacing: 1px;
            text-align: center;
        }

        .count{
            position: absolute;
            bottom: 5%;

            .score{
                font-weight: 600;
                color: var(--text-first);
            }
        }
    }
    
}

@media only screen and (min-width: 750px) {
    .game-display{
        width: 60%;
    }

    .btn-game{
        margin-top: 40px;
        
        width: 300px;

        font-weight: 600;
        font-size: 28px;
        line-height: 36px;
    }

    .title{
            font-size: 26px;
        }

    .score{
        font-size: 22px;
        margin-bottom: 20px;
    }
}

@media only screen and (max-width: 750px) {
    .game-display{
        width: 90%;
    }

    .btn-game{
        margin-top: 40px;
        
        width: 95%;

        font-weight: 600;
        font-size: 18px;
        line-height: 24px;
    }

    .title{
        font-size: 18px;
    }

    .score{
        font-size: 16px;
        margin-bottom: 20px;
    }
}
</style>