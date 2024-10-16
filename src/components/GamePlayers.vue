<template>
    <div class="vtb-item-players">
        <div>
            <div
                v-if="!!bracketNodeEntity.player1"
                :class="['vtb-player', 'vtb-player1', getPlayerClass(bracketNodeEntity.player1)]"
                @mouseover="highlightPlayer(bracketNodeEntity.player1.id)"
                @mouseleave="unhighlightPlayer"
            >
                <slot :player="bracketNodeEntity.player1" name="player" />
            </div>

            <div
                v-if="!!bracketNodeEntity.player2"
                :class="['vtb-player', 'vtb-player2', getPlayerClass(bracketNodeEntity.player2)]"
                @mouseover="highlightPlayer(bracketNodeEntity.player2.id)"
                @mouseleave="unhighlightPlayer"
            >
                <slot :player="bracketNodeEntity.player2" name="player" />
            </div>
        </div>
        <slot name="player-extension-bottom" :match="matchProperties" />
    </div>
</template>

<script>
    export default {
        name: "game-players",
        props: ["bracketNode", "highlightedPlayerId"],
        data() {
            const bracketNode = Object.assign({}, this.bracketNode)
            if(bracketNode?.player1 instanceof Object) {
                bracketNode.player1.round = this.bracketNode.round
            }
            if(bracketNode?.player2 instanceof Object) {
                bracketNode.player2.round = this.bracketNode.round
            }

            return {
                bracketNodeEntity: bracketNode,
            };
        },
        computed: {
            matchProperties() {
                return Object.assign({}, this.bracketNodeEntity, { games: undefined, hasParent: undefined });
            }
        },
        methods: {
            getPlayerClass(player) {
                if(!player) {
                    return ""
                }

                if (player.winner === null || player.winner === undefined) {
                    return "";
                }


                let clazz = player.winner ? "winner" : "defeated";

                if (this.highlightedPlayerId === player.id) {
                    clazz += " highlight";
                }

                return clazz;
            },
            highlightPlayer(playerId) {
                this.$emit("onSelectedPlayer", playerId);
            },
            unhighlightPlayer() {
                this.$emit("onDeselectedPlayer");
            }
        }
    };
</script>
