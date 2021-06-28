<template>
  <div id="space">
      <div ref="cont"></div>
  </div>
</template>

<script>
import * as THREE from 'three'
console.log(THREE)
import { MapControls } from 'three/examples/jsm/controls/OrbitControls'
export default {
    name: "HelloWorld",
    data() {
        return {
            scene: null,
            camera: null,
            renderer: null,
            controls: null,
            Window
        };
    },
    mounted(){
        this.render()
        let that = this
        window.addEventListener('resize',onWindowResize,false)

        function onWindowResize() {
            that.camera.aspect = window.innerWidth/window.innerHeight
            that.camera.updateProjectionMatrix()
            that.renderer.setSize(window.innerWidth,window.innerHeight)
        }

        onWindowResize()
    },
    methods:{
        render(){
            //获取渲染目标
            let cont = this.$refs.cont;
            
            //场景
            this.scene = new THREE.Scene()
            this.scene.background = new THREE.Color( 0x222222 )

            //相机
            this.camera = new THREE.PerspectiveCamera(25,window.clientWidth/window.clientHeight,1,100)
            this.camera.position.set(8,4,0);

            //光源
            let light0 = new THREE.AmbientLight(0xfafafa,0.25)

            let light1 = new THREE.PointLight(0xfafafa,0.4)
            light1.position.set(200,90,40);

            let light2 = new THREE.PointLight(0xfafafa,0.4)
            light2.position.set(200,90,-40);

            this.scene.add( light0 )
            this.scene.add( light1 )
            this.scene.add( light2 )

            // 坐标格辅助对象. 坐标格实际上是2维线数组
            let gh = new THREE.GridHelper( 80, 160, new THREE.Color( 0x555555 ), new THREE.Color( 0x333333 ))
            this.scene.add(gh)

            // 创建物体添加到场景
            let geometry = new THREE.BoxGeometry( 1, 1, 1)
            let material = new THREE.MeshBasicMaterial( {color: 0x00ff00 })
            let cube = new THREE.Mesh( geometry,material)
            this.scene.add( cube )

            //渲染器
            this.renderer = new THREE.WebGLRenderer({antialias:true})
            this.renderer.setPixelRatio(window.devicePixelRatio)
            this.renderer.setSize(window.innerWidth,window.innerHeight)
        
            cont.appendChild(this.renderer.domElement)

            // 控制器
            this.controls = new MapControls(this.camera,this.renderer.domElement)
            this.controls.enableDamping = true
            this.controls.dampingFactor = 0.25
            this.controls.screenSpacePanning = false
            this.controls.maxDistance = 800

            this.controls.update()

            this.Update()
        },
        Update(){
            requestAnimationFrame(this.Update)

            this.renderer.render(this.scene,this.camera)
            this.controls.update()
        }
    }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#space{
  width:100%;
  height:100%;
  margin:0;
  padding:0;
}
</style>
