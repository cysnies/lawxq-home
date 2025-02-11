<template>
    <div id="background-animation"></div>
  </template>
  
  <script setup>
  import { onMounted } from 'vue';
  
  let c1, c2, c3, c4;
  let N = 150;
  let n = 100;
  let t = 80;
  let b = 60;
  let th = 0;
  
  let back = [];
  let middle = [];
  let inter = [];
  let front = [];
  let points = [];
  
  let planet, planet2;
  let sz, msz;
  
  let viewport;
  
  function setup(p) {
      let density = p.displayDensity();
      p.pixelDensity(density);
      p.createCanvas(p.windowWidth, p.windowHeight);
      c1 = p.color("#140c35");
      c2 = p.color("#1dada4");
      c3 = p.color("#edf683");
      c4 = p.color("#fcfdef");
  
      viewport = p.min(p.windowHeight, p.windowWidth);
  
      planet = p.createVector(viewport / 4.1, -viewport / 5);
  
      sz = viewport / 7;
      msz = sz / 4;
  
      // 设置帧率为30FPS
      p.frameRate(30);
  
      init(p);
  }
  
  function windowResized(p) {
      p.resizeCanvas(p.windowWidth, p.windowHeight);
  
      viewport = p.min(p.windowHeight, p.windowWidth);
  
      planet = p.createVector(viewport / 4.1, -viewport / 5);
  
      sz = viewport / 7;
      msz = sz / 4;
  }
  
  function init(p) {
  
      for (let i = 0; i < N; i++) {
          let color;
          if (i / (N - 1) < 1 / 3) {
              color = p.lerpColor(c1, c2, i / (N / 3));
          } else if (i / (N - 1) < 2 / 3) {
              color = p.lerpColor(c2, c3, (i - N / 3) / (N / 3));
          } else {
              color = p.lerpColor(c3, c4, (i - 2 * N / 3) / (N / 3 - 1));
          }
          color.setAlpha(20 + 5 * i / (N - 1));
          back.push([color]);
      }
  
      for (let i = 0; i < b; i++) {
          let color;
          if (i / (b - 1) < 1 / 3) {
              color = p.lerpColor(c1, c2, p.min(i / (b / 3) + p.random(0, 0.15), 1));
          } else if (i / (b - 1) < 2 / 3) {
              color = p.lerpColor(c2, c3, p.min((i - b / 3) / (b / 3) + p.random(0, 0.15), 1));
          } else {
              color = p.lerpColor(c3, c4, p.min((i - 2 * b / 3) / (b / 3 - 1) + p.random(0, 0.15), 1));
          }
  
          color.setAlpha(70 + 5 * i / (b - 1));
          let r = p.random(5, 9);
          let k = 0;
          middle.push([]);
          for (let j = 0; j < r; j++) {
              let x = p.random(k, k + (p.TWO_PI - 0.01) / r / 2);
              let y = p.random(k + (p.TWO_PI - 0.01) / r / 2, k + (p.TWO_PI - 0.01) / r);
              if (y < x) {
                  let tmp = x;
                  x = y;
                  y = tmp;
              }
              let dir = 1;
              if (p.random(0, 1) < 0.5)
                  dir *= -1;
              middle[i].push([color, x, y, dir]);
              k += (p.TWO_PI - 0.01) / r;
          }
      }
  
      for (let i = 0; i < t; i++) {
          let color;
          if (i / (t - 1) < 1 / 3) {
              color = p.lerpColor(c1, c2, p.max(i / (t / 3) + p.random(0, -0.3), 0));
          } else if (i / (t - 1) < 2 / 3) {
              color = p.lerpColor(c2, c3, p.max((i - t / 3) / (t / 3) + p.random(0, -0.3), 0));
          } else {
              color = p.lerpColor(c3, c4, p.max((i - 2 * t / 3) / (t / 3 - 1) + p.random(0, -0.3), 0));
          }
  
          color.setAlpha(70 + 5 * i / (t - 1));
          let r = p.random(9, 15);
          let k = 0;
          inter.push([]);
          for (let j = 0; j < r; j++) {
              let x = p.random(k, k + (p.TWO_PI - 0.01) / r / 2);
              let y = p.random(k + (p.TWO_PI - 0.01) / r / 2, k + (p.TWO_PI - 0.01) / r);
              if (y < x) {
                  let tmp = x;
                  x = y;
                  y = tmp;
              }
              let dir = 1;
              if (p.random(0, 1) < 0.5)
                  dir *= -1;
              inter[i].push([color, x, y, dir]);
              k += (p.TWO_PI - 0.01) / r;
          }
      }
  
      for (let i = 0; i < n; i++) {
          let color;
          if (i / (n - 1) < 1 / 3) {
              color = p.lerpColor(c1, c2, p.min(i / (n / 3) + p.random(0, 0.15), 1));
          } else if (i / (n - 1) < 2 / 3) {
              color = p.lerpColor(c2, c3, p.min((i - n / 3) / (n / 3) + p.random(0, 0.15), 1));
          } else {
              color = p.lerpColor(c3, c4, p.min((i - 2 * n / 3) / (n / 3 - 1) + p.random(0, 0.15), 1));
          }
  
          color.setAlpha(155 + 100 * i / (n - 1));
  
          let r = p.random(3, 6);
          let k = 0;
          front.push([]);
          for (let j = 0; j < r; j++) {
              let x = p.random(k, k + (p.TWO_PI - 0.01) / r / 2);
              let y = p.random(k + (p.TWO_PI - 0.01) / r / 2, k + (p.TWO_PI - 0.01) / r);
              if (y < x) {
                  let tmp = x;
                  x = y;
                  y = tmp;
              }
              if (i % 4 < 1) continue;
              let dir = 1;
              if (p.random(0, 1) < 0.5)
                  dir *= -1;
              front[i].push([color, x, y, dir]);
              k += (p.TWO_PI - 0.01) / r;
          }
      }
  
      for (let i = 0; i < n; i++) {
          let color;
          if (i / (n - 1) < 1 / 3) {
              color = p.lerpColor(c1, c2, p.max(p.min(i / (n / 3) + p.random(-0.3, 0.3), 1), 0));
          } else if (i / (n - 1) < 2 / 3) {
              color = p.lerpColor(c2, c3, p.max(p.min((i - n / 3) / (n / 3) + p.random(-0.3, 0.3), 1), 0));
          } else {
              color = p.lerpColor(c3, c4, p.max(p.min((i - 2 * n / 3) / (n / 3 - 1) + p.random(-0.3, 0.3), 1), 0));
          }
  
          color.setAlpha(155 + 100 * p.random(-1, 1));
          let r = p.random(8, 16);
          let k = 0;
          points.push([]);
          for (let j = 0; j < r; j++) {
              let ang = p.random(k, k + (p.TWO_PI - 0.01) / r);
              let dir = p.random(-1, 1);
              points[i].push([color, ang, dir]);
              k += (p.TWO_PI - 0.01) / r;
          }
      }
  }
  
  function draw(p) {
      p.background("#140c25");
  
      p.scale(1, -1);
      p.translate(0, -p.windowHeight);
  
      p.translate(p.windowWidth / 2, p.windowHeight / 2);
  
      p.noStroke();
      for (let i = 0; i < N; i++) {
          let item = back[i];
          p.fill(item[0]);
          p.ellipse(0, 0, (viewport - 40) * (1 - i / (N + 1)));
      }
  
      p.noFill();
      p.strokeWeight((viewport - 40) / 2 / (b + 1));
      for (let i = 0; i < b; i++) {
          let group = middle[i];
          for (let j = 0; j < group.length; j++) {
              let item = group[j];
  
              p.stroke(item[0]);
              p.arc(0, 0, (viewport - 40) * (1 - i / (b + 1)), (viewport - 40) * (1 - i / (b + 1)), item[1] + th / 8 * (1.5 + (1 - i / (b + 1))) * item[3], item[2] + th / 8 * (1.5 + (1 - i / (b + 1))) * item[3]);
          }
      }
  
      p.strokeWeight((viewport - 40) / 2 / (t + 1));
      for (let i = 0; i < t; i++) {
          let group = inter[i];
          for (let j = 0; j < group.length; j++) {
              let item = group[j];
              p.stroke(item[0]);
              p.arc(0, 0, (viewport - 40) * (1 - i / (t + 1)), (viewport - 40) * (1 - i / (t + 1)), item[1] + th / 6 * (1.5 + (1 - i / (t + 1))) * item[3], item[2] + th / 6 * (1.5 + (1 - i / (t + 1))) * item[3]);
          }
      }
  
      p.strokeWeight((viewport - 40) / 2 / (n + 1));
      for (let i = 0; i < n; i++) {
          let group = front[i];
          for (let j = 0; j < group.length; j++) {
              let item = group[j];
              p.stroke(item[0]);
              p.arc(0, 0, (viewport - 40) * (1 - i / (n + 1)), (viewport - 40) * (1 - i / (n + 1)), item[1] + th / 2 * (1.5 + (1 - i / (n + 1)) / 2) * item[3], item[2] + th / 2 * (1.5 + (1 - i / (n + 1)) / 2) * item[3]);
          }
      }
  
      p.strokeWeight((viewport - 40) / (n + 1) / 2);
      for (let i = 0; i < n; i++) {
          let group = points[i];
          for (let j = 0; j < group.length; j++) {
              let item = group[j];
              p.stroke(item[0]);
              let r = (viewport) * (1 - i / (n + 1));
              p.arc(0, 0, r, r, item[1] + th / 2 * (1.5 + (1 - i / (n + 1)) / 2) * item[2], item[1] + th / 2 * (1.5 + (1 - i / (n + 1)) / 2) * item[2] + 0.0001);
          }
      }
  
      p.push();
      p.noStroke();
      p.rotate(2 * th / 3);
      // planet.set( mouseX - windowWidth/2, -mouseY + windowHeight/2, 0 );
      for (let i = 0; i < N; i++) {
  
          let j = i / (N - 1);
          p.push();
          p.translate(planet.x, planet.y);
          let norm = planet.copy().normalize();
          p.translate(norm.x * msz * j / 2 + 0.1, norm.y * msz * j / 2 + 0.1);
          p.rotate(planet.heading());
          let k1 = p.lerpColor(c2, c3, 0.5);
          let k2 = p.lerpColor(c1, c2, 0.2);
          let s = p.map(1 - (planet.mag() / viewport), 0, 0.999, 0, 0.5);
          let k = p.lerpColor(k1, k2, p.pow(j, s));
          k.setAlpha(25);
          p.fill(k);
          p.ellipse(0, 0, sz - msz * j, sz - msz * j * 0.36);
          p.pop();
      }
      p.pop();
  
      // 减慢旋转速度
      th += 0.005;
  
  }
  
  onMounted(() => {
      new p5((p) => {
          p.setup = () => setup(p);
          p.draw = () => draw(p);
          p.windowResized = () => windowResized(p);
      }, 'background-animation');
  });
  
  </script>
  
  <style lang="scss" scoped>
  #background-animation {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      z-index: -1; // 确保动画在最底层，不遮挡其他内容
      pointer-events: none; // 禁止用户交互
  }
  </style>