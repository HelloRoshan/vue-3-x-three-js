<template>
    <div ref="sceneContainer"></div>
</template>
<script>
import { Scene, PerspectiveCamera, BoxGeometry, MeshBasicMaterial, Mesh, WebGLRenderer } from 'three';
const scene = new Scene();
const camera = new PerspectiveCamera(
    75, window.innerWidth/window.innerHeight, 0.1, 500
);
const renderer = new WebGLRenderer()
const geometry = new BoxGeometry();
const material = new MeshBasicMaterial({
    color: 0x00ff00
});
const cube = new Mesh(geometry, material);
export default {
    name: 'MainContent',
    data() {
        return {
        };
    },
    created() {
        scene.add(cube);
        camera.position.z = 5;

        renderer.setSize(window.innerWidth, window.innerHeight);
    },
    mounted() {
        this.$refs.sceneContainer.appendChild(renderer.domElement);
        this.animate();
    },
    methods: {
        animate() {
            requestAnimationFrame(this.animate);

            cube.rotation.x += 0.01;
            cube.rotation.y += 0.01;

            renderer.render(scene, camera);
        }
    },
}
</script>