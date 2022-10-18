# example1事例

## 事例1

### 刷底色 


```javascript
// 给绘图区域刷颜色
  const canvas = document.getElementById("canvas");
  canvas.width=window.innerWidth;
  canvas.height=window.innerHeight;
  const gl = canvas.getContext("webgl");
  gl.clearColor(0, 0, 255, 1);
  gl.clear(gl.COLOR_BUFFER_BIT);
```

## 事例2

![示例图](/webgl/example1/assets/gif.gif?lang=zh-Hans)

```javascript
import { Color } from "https://unpkg.com/three/build/three.module.js";
const color = new Color(1, 0, 0);
const canvas = document.getElementById("canvas");
canvas.width=window.innerWidth;
canvas.height=window.innerHeight;
const gl = canvas.getContext("webgl");
!(function ani() {
  color.offsetHSL(0.005, 0, 0);
  gl.clearColor(color.r, color.g, color.b, 1);
  gl.clear(gl.COLOR_BUFFER_BIT);
  requestAnimationFrame(ani);
})();
```

## 事例3
![示例图](/webgl/example1/assets/example.png?lang=zh-Hans)

```html
<script id="vertexShader" type="x-shader/x-vertex">
    void main(){
      //点位
      gl_Position=vec4(0.0, 0.0, 0.0, 1.0);
      //尺寸
      gl_PointSize=50.0;
    }
  </script>
  <!-- 片元着色器 -->
  <script id="fragmentShader" type="x-shader/x-fragment">
    void main(){
      gl_FragColor=vec4(1,0,0,1);
    }
  </script>
```

```javascript
const canvas = document.getElementById("canvas");
canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

const gl = canvas.getContext("webgl");
const vsSource = document.getElementById('vertexShader').innerText;
const fsSource = document.getElementById('fragmentShader').innerText;

initShaders(gl, vsSource, fsSource);
gl.clearColor(0, 0, 0, 1);
gl.clear(gl.COLOR_BUFFER_BIT);
gl.drawArrays(gl.POINTS, 0, 1.0);

function initShaders(gl,vsSource,fsSource){
  //创建程序对象
  const program = gl.createProgram();
  //建立着色对象
  const vertexShader = loadShader(gl, gl.VERTEX_SHADER, vsSource);
  const fragmentShader = loadShader(gl, gl.FRAGMENT_SHADER, fsSource);
  //把顶点着色对象装进程序对象中
  gl.attachShader(program, vertexShader);
  //把片元着色对象装进程序对象中
  gl.attachShader(program, fragmentShader);
  //连接webgl上下文对象和程序对象
  gl.linkProgram(program);
  //启动程序对象
  gl.useProgram(program);
  //将程序对象挂到上下文对象上
  gl.program = program;
  return true;
}
function loadShader(gl, type, source) {
  //根据着色类型，建立着色器对象
  const shader = gl.createShader(type);
  //将着色器源文件传入着色器对象中
  gl.shaderSource(shader, source);
  //编译着色器对象
  gl.compileShader(shader);
  //返回着色器对象
  return shader;
}
```