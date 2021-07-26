<template>
    <TroisCanvas :cameraPosition="[2, 2, 5]">
        <mesh ref="parent">
            <meshBasicMaterial :wireframe="true" />

            <mesh :scale="0.5" ref="child" :visible="childVisible">
                <meshBasicMaterial color="red" :wireframe="true" />
            </mesh>
        </mesh>
    </TroisCanvas>
</template>

<script lang="ts">
import { defineComponent, watch } from 'vue'
import { useTrois } from 'trois-renderer-proof-of-concept'
const trois = useTrois()

export default defineComponent({
    data() {
        return {
            childVisible: false,
        }
    },
    mounted() {
        this.update()

        setInterval(() => (this.childVisible = !this.childVisible), 800)

        watch(
            () => trois.camera.value,
            (cam) => {
                if (cam) {
                    cam.lookAt(0, 0, 0)
                }
            },
            { immediate: true }
        )
    },
    methods: {
        update() {
            requestAnimationFrame(this.update)

            const newX = Math.sin(Date.now() * 0.001)

            const instance: THREE.Mesh = (this.$refs.parent as any).instance
            instance.position.x = newX

            if (!this.$refs.child) return

            const child: THREE.Mesh = (this.$refs.child as any).instance
            child.position.x = newX
            child.position.y = Math.cos(Date.now() * 0.001)
        },
    },
})
</script>