<template>
    <div ref="sceneContainer"></div>
</template>
<script>
import {
    Scene,
    PerspectiveCamera,
    BoxGeometry,
    Color,
    MeshBasicMaterial,
    Mesh,
    WebGLRenderer,
    LineBasicMaterial,
    BufferGeometry,
    Vector3,
    CircleGeometry,
    Group,
    Line,
} from 'three';
import { OrbitControls } from '../../node_modules/three/examples/jsm/controls/OrbitControls';

const scene = new Scene();
const camera = new PerspectiveCamera(
    75, window.innerWidth/window.innerHeight, 0.1, 500
);
const renderer = new WebGLRenderer()

const geometryCube = new BoxGeometry();
const materialCube = new MeshBasicMaterial({
    color: 0xff0000
});

const material = new MeshBasicMaterial({
    color: 0xa5a5a5
});

const materialLine = new LineBasicMaterial({
    color: 0xFF0000
});
const points = [];
points.push(new Vector3(-10, 0, 0));
points.push(new Vector3(0, 10, 0));
points.push(new Vector3(10, 0, 0));

const geometryLine = new BufferGeometry().setFromPoints(points);

const geometry = new CircleGeometry( 2, 100/* , 2, 1.2 * Math.PI */ );

const cube = new Mesh(geometryCube, materialCube);
const line = new Line(geometryLine, materialLine);
const circle = new Mesh(geometry, material);

const controls = new OrbitControls( camera, renderer.domElement );
export default {
    name: 'MainContent',
    data() {
        return {

        };
    },
    created() {
        renderer.setSize(window.innerWidth, window.innerHeight);
        camera.position.set(0, 0, 5);
        controls.update();
        scene.background = new Color('#000');

        const group = new Group();
        group.add(cube);
        group.add(circle);
        group.add(line);
        scene.add(group);
    },
    mounted() {
        this.$refs.sceneContainer.appendChild(renderer.domElement);
        this.animate();
    },
    methods: {
        animate() {
            requestAnimationFrame(this.animate);
            controls.update();

            circle.rotation.x += 0.01;
            circle.rotation.y += 0.01;
            cube.rotation.x += 0.01;
            cube.rotation.y += 0.01;

            renderer.render(scene, camera);
        }
    },
}
</script>