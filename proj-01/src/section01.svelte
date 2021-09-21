<script>

let canvas, ctx;
let render, init;
let blob;

class Blob {
  constructor() {
    this.points = [];
  }

  init() {
    for (let i = 0; i < this.numPoints; i++) {
      let point = new Point(this.divisional * (i + 1), this);
      // point.acceleration = -1 + Math.random() * 2;
      this.push(point);
    }
  }

  render() {
    let canvas = this.canvas;
    let ctx = this.ctx;
    let position = this.position;
    let pointsArray = this.points;
    let radius = this.radius;
    let points = this.numPoints;
    let divisional = this.divisional;
    let center = this.center;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    pointsArray[0].solveWith(pointsArray[points - 1], pointsArray[1]);

    let p0 = pointsArray[points - 1].position;
    let p1 = pointsArray[0].position;
    let _p2 = p1;

    ctx.beginPath();
    ctx.moveTo(center.x, center.y);
    ctx.moveTo((p0.x + p1.x) / 2, (p0.y + p1.y) / 2);

    for (let i = 1; i < points; i++) {
      pointsArray[i].solveWith(
        pointsArray[i - 1],
        pointsArray[i + 1] || pointsArray[0]
      );

      let p2 = pointsArray[i].position;
      var xc = (p1.x + p2.x) / 2;
      var yc = (p1.y + p2.y) / 2;
      ctx.quadraticCurveTo(p1.x, p1.y, xc, yc);
      // ctx.lineTo(p2.x, p2.y);

      ctx.fillStyle = "#000000";
      // ctx.fillRect(p1.x-2.5, p1.y-2.5, 5, 5);

      p1 = p2;
    }

    var xc = (p1.x + _p2.x) / 2;
    var yc = (p1.y + _p2.y) / 2;
    ctx.quadraticCurveTo(p1.x, p1.y, xc, yc);
    // ctx.lineTo(_p2.x, _p2.y);

    // ctx.closePath();
    ctx.fillStyle = this.color;
    ctx.fill();
    ctx.strokeStyle = "#000000";
    // ctx.stroke();

    /*
        ctx.fillStyle = '#000000';
        if(this.mousePos) {
          let angle = Math.atan2(this.mousePos.y, this.mousePos.x) + Math.PI;
          ctx.fillRect(center.x + Math.cos(angle) * this.radius, center.y + Math.sin(angle) * this.radius, 5, 5);
        }
    */
    requestAnimationFrame(this.render.bind(this));
  }

  push(item) {
    if (item instanceof Point) {
      this.points.push(item);
    }
  }

  set color(value) {
    this._color = value;
  }
  get color() {
    return this._color || "#FFFFFF";
  }

  set canvas(value) {
    if (
      value instanceof HTMLElement &&
      value.tagName.toLowerCase() === "canvas"
    ) {
      this._canvas = canvas;
      this.ctx = this._canvas.getContext("2d");
    }
  }
  get canvas() {
    return this._canvas;
  }

  set numPoints(value) {
    if (value > 2) {
      this._points = value;
    }
  }
  get numPoints() {
    return this._points || 32;
  }

  set radius(value) {
    if (value > 0) {
      this._radius = value;
    }
  }
  get radius() {
    return this._radius || 150;
  }

  set position(value) {
    if (typeof value == "object" && value.x && value.y) {
      this._position = value;
    }
  }
  get position() {
    return this._position || { x: 0.5, y: 0.5 };
  }

  get divisional() {
    return (Math.PI * 2) / this.numPoints;
  }

  get center() {
    return {
      x: this.canvas.width * this.position.x,
      y: this.canvas.height * this.position.y
    };
  }

  set running(value) {
    this._running = value === true;
  }
  get running() {
    return this.running !== false;
  }
}

class Point {
  constructor(azimuth, parent) {
    this.parent = parent;
    this.azimuth = Math.PI - azimuth;
    this._components = {
      x: Math.cos(this.azimuth),
      y: Math.sin(this.azimuth)
    };

    this.acceleration = -0.3 + Math.random() * 0.6;
  }

  solveWith(leftPoint, rightPoint) {
    this.acceleration =
      (-0.3 * this.radialEffect +
        (leftPoint.radialEffect - this.radialEffect) +
        (rightPoint.radialEffect - this.radialEffect)) *
        this.elasticity -
      this.speed * this.friction;
  }

  set acceleration(value) {
    if (typeof value == "number") {
      this._acceleration = value;
      this.speed += this._acceleration * 2;
    }
  }
  get acceleration() {
    return this._acceleration || 0;
  }

  set speed(value) {
    if (typeof value == "number") {
      this._speed = value;
      this.radialEffect += this._speed * 5;
    }
  }
  get speed() {
    return this._speed || 0;
  }

  set radialEffect(value) {
    if (typeof value == "number") {
      this._radialEffect = value;
    }
  }
  get radialEffect() {
    return this._radialEffect || 0;
  }

  get position() {
    return {
      x:
        this.parent.center.x +
        this.components.x * (this.parent.radius + this.radialEffect),
      y:
        this.parent.center.y +
        this.components.y * (this.parent.radius + this.radialEffect)
    };
  }

  get components() {
    return this._components;
  }

  set elasticity(value) {
    if (typeof value === "number") {
      this._elasticity = value;
    }
  }
  get elasticity() {
    return this._elasticity || 0.001;
  }
  set friction(value) {
    if (typeof value === "number") {
      this._friction = value;
    }
  }
  get friction() {
    return this._friction || 0.0085;
  }
}

blob = new Blob();

init = function () {
  canvas = document.createElement("canvas");
  canvas.setAttribute("touch-action", "none");

  document.body.appendChild(canvas);

  let resize = function () {
    canvas.width = 400;
    canvas.height = window.innerHeight;
  };
  window.addEventListener("resize", resize);
  resize();

  let oldMousePoint = { x: 0, y: 0 };
  let hover = false;
  let mouseMove = function (e) {
    let pos = blob.center;
    let diff = { x: e.clientX - pos.x, y: e.clientY - pos.y };
    let dist = Math.sqrt(diff.x * diff.x + diff.y * diff.y);
    let angle = null;

    blob.mousePos = { x: pos.x - e.clientX, y: pos.y - e.clientY };

    if (dist < blob.radius && hover === false) {
      let vector = { x: e.clientX - pos.x, y: e.clientY - pos.y };
      angle = Math.atan2(vector.y, vector.x);
      hover = true;
      // blob.color = '#77FF00';
    } else if (dist > blob.radius && hover === true) {
      let vector = { x: e.clientX - pos.x, y: e.clientY - pos.y };
      angle = Math.atan2(vector.y, vector.x);
      hover = false;
      blob.color = null;
    }

    if (typeof angle == "number") {
      let nearestPoint = null;
      let distanceFromPoint = 100;

      blob.points.forEach((point) => {
        if (Math.abs(angle - point.azimuth) < distanceFromPoint) {
          // console.log(point.azimuth, angle, distanceFromPoint);
          nearestPoint = point;
          distanceFromPoint = Math.abs(angle - point.azimuth);
        }
      });

      if (nearestPoint) {
        let strength = {
          x: oldMousePoint.x - e.clientX,
          y: oldMousePoint.y - e.clientY
        };
        strength =
          Math.sqrt(strength.x * strength.x + strength.y * strength.y) * 10;
        if (strength > 100) strength = 100;
        nearestPoint.acceleration = (strength / 100) * (hover ? -1 : 1);
      }
    }

    oldMousePoint.x = e.clientX;
    oldMousePoint.y = e.clientY;
  };
  // window.addEventListener('mousemove', mouseMove);
  window.addEventListener("pointermove", mouseMove);

  blob.canvas = canvas;
  blob.init();
  blob.render();
};

init();


</script>

<section class="hero-section" id="one">
    <div class="grid-container">
      <div class="grid-pt-01">
        <svg width="100%" height="100%" viewBox="0 0 1331 14" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M18.9626 0H0.137695V12.5301H18.9626V0Z" fill="#D3D3D3" />
          <path d="M156.228 0H50.3374V12.5301H156.228V0Z" fill="#D3D3D3" />
          <path d="M197.015 0H187.603V12.5301H197.015V0Z" fill="#D3D3D3" />
          <path d="M656.406 10.9967L656.717 10.2774L656.712 10.2754L656.406 10.9967ZM655.023 10.0758L654.463 10.6244L654.468 10.6295L654.473 10.6346L655.023 10.0758ZM654.089 8.68337L653.369 8.9931L654.089 8.68337ZM654.089 5.3065L653.369 4.9968L654.089 5.3065ZM655.023 3.92506L654.468 3.37129L655.023 3.92506ZM656.406 2.99313L656.712 3.71453L656.717 3.71242L656.406 2.99313ZM659.788 2.99313L659.479 3.71247L659.487 3.71602L659.788 2.99313ZM661.172 3.92506L660.613 4.47371L660.617 4.47884L660.623 4.48387L661.172 3.92506ZM662.105 5.3065L661.381 5.60773L661.385 5.6162L662.105 5.3065ZM662.105 8.68337L661.385 8.37364L661.381 8.38218L662.105 8.68337ZM661.172 10.0758L661.727 10.6295L661.172 10.0758ZM659.788 10.9967L659.487 10.2738L659.479 10.2774L659.788 10.9967ZM659.492 10.306L659.799 11.0271L659.803 11.0249L659.809 11.0227L659.492 10.306ZM660.634 9.52759L660.079 8.97383L660.634 9.52759ZM661.403 8.38735L660.685 8.07151L660.683 8.07668L660.681 8.08184L661.403 8.38735ZM661.403 5.60253L660.681 5.90806L660.683 5.91321L660.685 5.91833L661.403 5.60253ZM660.634 4.46228L661.189 3.90852L660.634 4.46228ZM659.492 3.69481L659.176 4.41145L659.181 4.4137L659.186 4.41588L659.492 3.69481ZM656.703 3.69481L657.009 4.41588L657.014 4.4137L657.02 4.41145L656.703 3.69481ZM655.561 4.46228L655.006 3.90852L655.561 4.46228ZM654.781 5.60253L654.063 5.28672L654.062 5.29184L654.059 5.29699L654.781 5.60253ZM654.781 8.38735L654.059 8.69284L654.062 8.69801L654.063 8.70318L654.781 8.38735ZM655.561 9.52759L656.116 8.97383L655.561 9.52759ZM656.703 10.306L656.387 11.0227L656.392 11.0249L656.397 11.0271L656.703 10.306ZM656.9 9.82361L656.585 10.5404L656.594 10.5442L656.602 10.5478L656.9 9.82361ZM655.923 9.16578L655.361 9.71233L655.368 9.71961L655.376 9.72666L655.923 9.16578ZM655.264 8.19L654.539 8.48813L654.542 8.49683L654.546 8.50536L655.264 8.19ZM655.264 5.81084L654.544 5.50063L654.542 5.50759L655.264 5.81084ZM655.923 4.83506L656.478 5.38882L655.923 4.83506ZM656.9 4.17722L657.205 4.89929L657.212 4.8963L656.9 4.17722ZM659.294 4.17722L658.984 4.8963L658.99 4.89889L658.996 4.90137L659.294 4.17722ZM660.272 4.83506L659.71 5.38157L659.717 5.38886L659.725 5.39596L660.272 4.83506ZM660.93 5.81084L660.206 6.10902L660.209 6.11767L660.213 6.12624L660.93 5.81084ZM660.93 8.19L660.21 7.8798L660.208 7.88677L660.93 8.19ZM660.272 9.16578L660.827 9.71953L660.272 9.16578ZM659.294 9.82361L659.593 10.5478L659.599 10.5453L659.606 10.5427L659.294 9.82361ZM659.261 8.99036L658.867 8.31389L658.86 8.31757L659.261 8.99036ZM660.096 8.1571L659.425 7.75213L659.422 7.7573L659.419 7.76251L660.096 8.1571ZM660.096 5.83277L659.419 6.22736L659.424 6.23574L659.428 6.244L660.096 5.83277ZM659.261 4.99951L658.86 5.67233L658.867 5.67597L659.261 4.99951ZM656.099 5.83277L656.773 6.2336L656.776 6.22736L656.099 5.83277ZM656.099 8.1571L655.422 8.55172L656.099 8.1571ZM658.097 10.5535C657.599 10.5535 657.141 10.46 656.717 10.2774L656.096 11.7161C656.726 11.9867 657.396 12.1198 658.097 12.1198V10.5535ZM656.712 10.2754C656.283 10.0944 655.905 9.84264 655.572 9.51694L654.473 10.6346C654.946 11.0983 655.491 11.4605 656.101 11.7181L656.712 10.2754ZM655.583 9.52712C655.256 9.19538 654.999 8.81259 654.81 8.37364L653.369 8.9931C653.633 9.60668 653.999 10.1522 654.463 10.6244L655.583 9.52712ZM654.81 8.37364C654.627 7.94973 654.533 7.4928 654.533 6.99494H652.964C652.964 7.69579 653.098 8.36448 653.369 8.9931L654.81 8.37364ZM654.533 6.99494C654.533 6.49707 654.627 6.04016 654.81 5.6162L653.369 4.9968C653.098 5.62538 652.964 6.29408 652.964 6.99494H654.533ZM654.81 5.6162C654.999 5.17858 655.255 4.80151 655.578 4.47882L654.468 3.37129C654.001 3.838 653.634 4.3819 653.369 4.9968L654.81 5.6162ZM655.578 4.47882C655.91 4.14754 656.286 3.89411 656.712 3.71451L656.101 2.27175C655.488 2.5307 654.942 2.89856 654.468 3.37129L655.578 4.47882ZM656.717 3.71242C657.141 3.52989 657.599 3.43638 658.097 3.43638V1.87012C657.396 1.87012 656.726 2.00319 656.096 2.27384L656.717 3.71242ZM658.097 3.43638C658.596 3.43638 659.054 3.52989 659.479 3.71242L660.099 2.27384C659.469 2.00319 658.799 1.87012 658.097 1.87012V3.43638ZM659.487 3.71602C659.919 3.89552 660.292 4.14733 660.613 4.47371L661.732 3.3764C661.262 2.89876 660.713 2.52929 660.09 2.27023L659.487 3.71602ZM660.623 4.48387C660.95 4.80432 661.202 5.17691 661.381 5.6077L662.83 5.00529C662.571 4.38356 662.2 3.83519 661.722 3.36624L660.623 4.48387ZM661.385 5.6162C661.568 6.04016 661.661 6.49707 661.661 6.99494H663.23C663.23 6.29408 663.098 5.62538 662.826 4.9968L661.385 5.6162ZM661.661 6.99494C661.661 7.4928 661.568 7.94973 661.385 8.37364L662.826 8.9931C663.098 8.36448 663.23 7.69579 663.23 6.99494H661.661ZM661.381 8.38218C661.202 8.81431 660.948 9.19256 660.617 9.52203L661.727 10.6295C662.203 10.155 662.571 9.60496 662.83 8.98456L661.381 8.38218ZM660.617 9.52203C660.297 9.8428 659.922 10.0931 659.487 10.2738L660.09 11.7197C660.71 11.4618 661.258 11.0982 661.727 10.6295L660.617 9.52203ZM659.479 10.2774C659.054 10.46 658.596 10.5535 658.097 10.5535V12.1198C658.799 12.1198 659.469 11.9867 660.099 11.7161L659.479 10.2774ZM658.097 11.3632C658.694 11.3632 659.264 11.2532 659.799 11.0271L659.186 9.58491C658.857 9.72423 658.496 9.79698 658.097 9.79698V11.3632ZM659.809 11.0227C660.328 10.7941 660.79 10.4797 661.189 10.0813L660.079 8.97383C659.82 9.23328 659.52 9.43792 659.176 9.58937L659.809 11.0227ZM661.189 10.0813C661.59 9.68069 661.903 9.21621 662.125 8.69284L660.681 8.08184C660.537 8.42094 660.337 8.71665 660.079 8.97383L661.189 10.0813ZM662.121 8.70318C662.357 8.16697 662.472 7.59465 662.472 6.99494H660.904C660.904 7.38928 660.829 7.7452 660.685 8.07151L662.121 8.70318ZM662.472 6.99494C662.472 6.39522 662.357 5.82288 662.121 5.28672L660.685 5.91833C660.829 6.24467 660.904 6.60059 660.904 6.99494H662.472ZM662.125 5.29699C661.903 4.77367 661.59 4.30916 661.189 3.90852L660.079 5.01605C660.337 5.27324 660.537 5.56889 660.681 5.90806L662.125 5.29699ZM661.189 3.90852C660.788 3.5079 660.323 3.19549 659.799 2.97375L659.186 4.41588C659.526 4.5596 659.822 4.75884 660.079 5.01605L661.189 3.90852ZM659.809 2.97818C659.271 2.74191 658.698 2.62662 658.097 2.62662V4.19289C658.493 4.19289 658.849 4.26764 659.176 4.41145L659.809 2.97818ZM658.097 2.62662C657.497 2.62662 656.924 2.74191 656.387 2.97818L657.02 4.41145C657.346 4.26764 657.703 4.19289 658.097 4.19289V2.62662ZM656.397 2.97375C655.873 3.19549 655.408 3.5079 655.006 3.90852L656.116 5.01605C656.373 4.75884 656.67 4.5596 657.009 4.41588L656.397 2.97375ZM655.006 3.90852C654.608 4.30691 654.292 4.76795 654.063 5.28672L655.499 5.91833C655.651 5.57461 655.856 5.27549 656.116 5.01605L655.006 3.90852ZM654.059 5.29699C653.833 5.83072 653.722 6.39945 653.722 6.99494H655.291C655.291 6.59636 655.364 6.23682 655.503 5.90806L654.059 5.29699ZM653.722 6.99494C653.722 7.59042 653.833 8.15914 654.059 8.69284L655.503 8.08184C655.364 7.75305 655.291 7.39351 655.291 6.99494H653.722ZM654.063 8.70318C654.292 9.22193 654.608 9.68296 655.006 10.0813L656.116 8.97383C655.856 8.71438 655.651 8.41522 655.499 8.07151L654.063 8.70318ZM655.006 10.0813C655.405 10.4797 655.867 10.7941 656.387 11.0227L657.02 9.58937C656.675 9.43792 656.376 9.23328 656.116 8.97383L655.006 10.0813ZM656.397 11.0271C656.932 11.2532 657.501 11.3632 658.097 11.3632V9.79698C657.699 9.79698 657.338 9.72423 657.009 9.58491L656.397 11.0271ZM658.097 9.27072C657.77 9.27072 657.473 9.21198 657.199 9.09945L656.602 10.5478C657.075 10.7422 657.576 10.837 658.097 10.837V9.27072ZM657.216 9.10681C656.936 8.98362 656.689 8.81705 656.47 8.6049L655.376 9.72666C655.729 10.07 656.133 10.342 656.585 10.5404L657.216 9.10681ZM656.485 8.61923C656.273 8.40152 656.106 8.1546 655.983 7.87463L654.546 8.50536C654.746 8.95629 655.018 9.35992 655.361 9.71233L656.485 8.61923ZM655.99 7.89178C655.877 7.61846 655.818 7.32194 655.818 6.99494H654.249C654.249 7.5158 654.345 8.01598 654.539 8.48813L655.99 7.89178ZM655.818 6.99494C655.818 6.66781 655.877 6.377 655.987 6.11409L654.542 5.50759C654.345 5.97559 654.249 6.47418 654.249 6.99494H655.818ZM655.985 6.12103C656.108 5.83606 656.272 5.5942 656.478 5.38882L655.368 4.28129C655.018 4.63141 654.743 5.04007 654.544 5.50065L655.985 6.12103ZM656.478 5.38882C656.693 5.1743 656.934 5.01275 657.205 4.89926L656.597 3.45519C656.136 3.64869 655.725 3.92569 655.368 4.28129L656.478 5.38882ZM657.212 4.8963C657.482 4.77963 657.775 4.71915 658.097 4.71915V3.15289C657.571 3.15289 657.066 3.25321 656.59 3.45814L657.212 4.8963ZM658.097 4.71915C658.42 4.71915 658.713 4.77963 658.984 4.8963L659.606 3.45814C659.13 3.25321 658.624 3.15289 658.097 3.15289V4.71915ZM658.996 4.90137C659.271 5.01473 659.508 5.17403 659.71 5.38157L660.834 4.28854C660.48 3.92596 660.064 3.64671 659.593 3.45308L658.996 4.90137ZM659.725 5.39596C659.933 5.59818 660.092 5.83371 660.206 6.10902L661.656 5.51266C661.462 5.04243 661.182 4.62743 660.819 4.27415L659.725 5.39596ZM660.213 6.12624C660.327 6.38624 660.388 6.67277 660.388 6.99494H661.956C661.956 6.46923 661.856 5.96636 661.649 5.49544L660.213 6.12624ZM660.388 6.99494C660.388 7.31701 660.327 7.60932 660.21 7.8798L661.651 8.50019C661.856 8.02515 661.956 7.52074 661.956 6.99494H660.388ZM660.208 7.88677C660.094 8.15695 659.932 8.39752 659.717 8.61203L660.827 9.71953C661.183 9.36391 661.46 8.95394 661.654 8.49322L660.208 7.88677ZM659.717 8.61203C659.511 8.81744 659.269 8.98159 658.984 9.10454L659.606 10.5427C660.067 10.344 660.476 10.0697 660.827 9.71953L659.717 8.61203ZM658.996 9.09945C658.722 9.21198 658.425 9.27072 658.097 9.27072V10.837C658.62 10.837 659.12 10.7422 659.593 10.5478L658.996 9.09945ZM658.097 10.0805C658.66 10.0805 659.188 9.9457 659.663 9.66315L658.86 8.31757C658.647 8.44436 658.4 8.51421 658.097 8.51421V10.0805ZM659.657 9.66683C660.125 9.39406 660.501 9.01925 660.774 8.55172L659.419 7.76251C659.282 7.99664 659.101 8.17731 658.867 8.31389L659.657 9.66683ZM660.767 8.56206C661.057 8.08474 661.199 7.5565 661.199 6.99494H659.63C659.63 7.28125 659.561 7.52779 659.425 7.75213L660.767 8.56206ZM661.199 6.99494C661.199 6.42878 661.057 5.89719 660.764 5.42154L659.428 6.244C659.559 6.45541 659.63 6.69859 659.63 6.99494H661.199ZM660.774 5.43817C660.501 4.97064 660.125 4.59579 659.657 4.32306L658.867 5.67597C659.101 5.81255 659.282 5.99321 659.419 6.22736L660.774 5.43817ZM659.663 4.32672C659.188 4.04419 658.66 3.90939 658.097 3.90939V5.47566C658.4 5.47566 658.647 5.54551 658.86 5.6723L659.663 4.32672ZM658.097 3.90939C657.54 3.90939 657.015 4.04542 656.539 4.32306L657.329 5.67597C657.555 5.54429 657.805 5.47566 658.097 5.47566V3.90939ZM656.539 4.32306C656.07 4.59579 655.695 4.97064 655.422 5.43817L656.776 6.22736C656.914 5.99321 657.094 5.81255 657.329 5.67597L656.539 4.32306ZM655.425 5.43195C655.143 5.9062 655.007 6.43405 655.007 6.99494H656.576C656.576 6.69333 656.646 6.44641 656.773 6.23358L655.425 5.43195ZM655.007 6.99494C655.007 7.5512 655.143 8.07574 655.422 8.55172L656.776 7.76251C656.645 7.53678 656.576 7.28653 656.576 6.99494H655.007ZM655.422 8.55172C655.695 9.01925 656.07 9.39406 656.539 9.66683L657.329 8.31389C657.094 8.17731 656.914 7.99664 656.776 7.76251L655.422 8.55172ZM656.539 9.66683C657.015 9.94445 657.54 10.0805 658.097 10.0805V8.51421C657.805 8.51421 657.555 8.44561 657.329 8.31389L656.539 9.66683ZM671.292 12.5098L670.738 13.0641L671.293 13.6167L671.847 13.0635L671.292 12.5098ZM665.757 6.99494L665.203 6.44117L664.648 6.99548L665.204 7.54924L665.757 6.99494ZM671.292 1.46915L671.847 0.915939L671.293 0.361084L670.737 0.915391L671.292 1.46915ZM676.816 6.99494L677.37 7.5487L677.925 6.99548L677.371 6.44173L676.816 6.99494ZM671.292 11.512L670.737 12.0658L671.292 12.6196L671.847 12.0658L671.292 11.512ZM675.816 6.99494L676.371 7.5487L676.925 6.99561L676.372 6.44185L675.816 6.99494ZM671.292 2.46686L671.847 1.91378L671.292 1.358L670.737 1.91378L671.292 2.46686ZM666.768 6.99494L666.212 6.44185L665.659 6.99561L666.213 7.5487L666.768 6.99494ZM671.847 11.9555L666.312 6.44063L665.204 7.54924L670.738 13.0641L671.847 11.9555ZM666.312 7.5487L671.847 2.02291L670.737 0.915391L665.203 6.44117L666.312 7.5487ZM670.737 2.02236L676.26 7.54814L677.371 6.44173L671.847 0.915939L670.737 2.02236ZM676.261 6.44117L670.737 11.956L671.847 13.0635L677.37 7.5487L676.261 6.44117ZM671.847 12.0658L676.371 7.5487L675.262 6.44117L670.737 10.9583L671.847 12.0658ZM676.372 6.44185L671.847 1.91378L670.737 3.01995L675.261 7.54802L676.372 6.44185ZM670.737 1.91378L666.212 6.44185L667.323 7.54802L671.847 3.01995L670.737 1.91378ZM666.213 7.5487L670.737 12.0658L671.847 10.9583L667.322 6.44117L666.213 7.5487Z" fill="white" />
          <path d="M1142.97 0H1133.55V12.5301H1142.97V0Z" fill="#D3D3D3" />
          <path d="M1280.23 0H1174.34V12.5301H1280.23V0Z" fill="#D3D3D3" />
          <path d="M1330.43 0H1311.61V12.5301H1330.43V0Z" fill="#D3D3D3" />
        </svg>
      </div>
      <div class="grid">
        
            <div class="center-text"> ZAC </div>
        <div class="text-container">
          
          <div class="top-left-el">
            <div class="title"> Portfolio </div>
            <div class="sub-title">27-21012-2421</div>
          </div>
  
          <div class="el-01">
            <div class="ripple"></div>
            <div class="connecting-01"> connecting... </div>
          </div>
        </div>
        <div class="grid-svg"> 
       <svg width="100%" height="100%" viewBox="0 0 1337 639" fill="none" xmlns="http://www.w3.org/2000/svg" style="">
  <g style="mix-blend-mode:hard-light" opacity="0.7">
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 4.55425L1.78418 4.55417V3.77112L1328.94 3.7712V4.55425Z" stroke="white" class="AtFSnXJu_0"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 67.988H1.78418V67.2048H1328.94V67.988Z" stroke="white" class="AtFSnXJu_1"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 130.639H1.78418H1328.94Z" stroke="white" class="AtFSnXJu_2"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 194.072H1.78418V193.289H1328.94V194.072Z" stroke="white" class="AtFSnXJu_3"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 256.724L1.78418 256.723L1328.94 256.724Z" stroke="white" class="AtFSnXJu_4"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 320.157L1.78418 320.157V319.374H1328.94V320.157Z" stroke="white" class="AtFSnXJu_5"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 382.808H1.78418H1328.94Z" stroke="white" class="AtFSnXJu_6"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 446.242H1.78418V445.459H1328.94V446.242Z" stroke="white" class="AtFSnXJu_7"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 509.675H1.78418V508.892H1328.94V509.675Z" stroke="white" class="AtFSnXJu_8"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 573.109H1.78418V572.326H1328.94V573.109Z" stroke="white" class="AtFSnXJu_9"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1328.94 636.543H1.78418V635.76H1328.94V636.543Z" stroke="white" class="AtFSnXJu_10"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1.78418 636.542L1.78433 4.5542H2.56854L2.56839 636.542H1.78418Z" stroke="white" stroke-width="2" class="AtFSnXJu_11"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M65.3184 636.542L65.3185 4.5542H66.1027L66.1026 636.542H65.3184Z" stroke="white" stroke-width="2" class="AtFSnXJu_12"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M128.853 636.542V4.5542V636.542Z" stroke="white" stroke-width="2" class="AtFSnXJu_13"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M191.602 636.542L191.603 4.5542H192.386V636.542H191.602Z" stroke="white" stroke-width="2" class="AtFSnXJu_14"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M255.136 636.542L255.137 4.5542H255.921V636.542H255.136Z" stroke="white" stroke-width="2" class="AtFSnXJu_15"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M318.67 636.542L318.671 4.5542H319.455V636.542H318.67Z" stroke="white" stroke-width="2" class="AtFSnXJu_16"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M382.205 636.542L382.205 4.5542H382.205V636.542Z" stroke="white" stroke-width="2" class="AtFSnXJu_17"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M444.955 636.542V4.5542H445.739V636.542H444.955Z" stroke="white" stroke-width="2" class="AtFSnXJu_18"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M508.489 636.542V4.5542H509.273V636.542H508.489Z" stroke="white" stroke-width="2" class="AtFSnXJu_19"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M572.023 636.542V4.5542H572.807V636.542H572.023Z" stroke="white" stroke-width="2" class="AtFSnXJu_20"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M635.557 636.542V4.5542H636.341V636.542H635.557Z" stroke="white" stroke-width="2" class="AtFSnXJu_21"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M699.091 636.542V4.5542V636.542Z" stroke="white" stroke-width="2" class="AtFSnXJu_22"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M761.841 636.542V4.5542H762.625V636.542H761.841Z" stroke="white" stroke-width="2" class="AtFSnXJu_23"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M825.374 636.542V4.5542H826.158V636.542H825.374Z" stroke="white" stroke-width="2" class="AtFSnXJu_24"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M888.908 636.542V4.5542H889.693V636.542H888.908Z" stroke="white" stroke-width="2" class="AtFSnXJu_25"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M952.442 636.542V4.5542V636.542Z" stroke="white" stroke-width="2" class="AtFSnXJu_26"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1015.19 636.542V4.5542H1015.98V636.542H1015.19Z" stroke="white" stroke-width="2" class="AtFSnXJu_27"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1078.73 636.542V4.5542H1079.51V636.542H1078.73Z" stroke="white" stroke-width="2" class="AtFSnXJu_28"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1142.26 636.542V4.5542H1143.04V636.542H1142.26Z" stroke="white" stroke-width="2" class="AtFSnXJu_29"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1205.79 636.542V4.5542H1206.58V636.542H1205.79Z" stroke="white" stroke-width="2" class="AtFSnXJu_30"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1269.33 636.542V4.5542V636.542Z" stroke="white" stroke-width="2" class="AtFSnXJu_31"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path fill-rule="evenodd" clip-rule="evenodd" d="M1332.08 636.542V4.5542H1332.86V636.542H1332.08Z" stroke="white" stroke-width="2" class="AtFSnXJu_32"></path>
  </g>
  <g style="mix-blend-mode:hard-light" opacity="0.2">
  <path d="M1334.43 2.20483H3.35303V636.542H1334.43V2.20483Z" stroke="white" stroke-width="4" class="AtFSnXJu_33"></path>
  </g>
  </g>
  <style data-made-with="vivus-instant">.AtFSnXJu_0{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_0 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_1{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_1 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_2{stroke-dasharray:2655 2657;stroke-dashoffset:2656;animation:AtFSnXJu_draw_2 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_3{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_3 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_4{stroke-dasharray:2655 2657;stroke-dashoffset:2656;animation:AtFSnXJu_draw_4 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_5{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_5 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_6{stroke-dasharray:2655 2657;stroke-dashoffset:2656;animation:AtFSnXJu_draw_6 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_7{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_7 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_8{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_8 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_9{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_9 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_10{stroke-dasharray:2656 2658;stroke-dashoffset:2657;animation:AtFSnXJu_draw_10 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_11{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_11 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_12{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_12 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_13{stroke-dasharray:1264 1266;stroke-dashoffset:1265;animation:AtFSnXJu_draw_13 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_14{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_14 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_15{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_15 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_16{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_16 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_17{stroke-dasharray:1264 1266;stroke-dashoffset:1265;animation:AtFSnXJu_draw_17 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_18{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_18 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_19{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_19 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_20{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_20 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_21{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_21 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_22{stroke-dasharray:1264 1266;stroke-dashoffset:1265;animation:AtFSnXJu_draw_22 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_23{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_23 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_24{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_24 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_25{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_25 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_26{stroke-dasharray:1264 1266;stroke-dashoffset:1265;animation:AtFSnXJu_draw_26 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_27{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_27 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_28{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_28 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_29{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_29 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_30{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_30 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_31{stroke-dasharray:1264 1266;stroke-dashoffset:1265;animation:AtFSnXJu_draw_31 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_32{stroke-dasharray:1266 1268;stroke-dashoffset:1267;animation:AtFSnXJu_draw_32 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}.AtFSnXJu_33{stroke-dasharray:3931 3933;stroke-dashoffset:3932;animation:AtFSnXJu_draw_33 5900ms ease-out 0ms infinite,AtFSnXJu_fade 5900ms linear 0ms infinite;}@keyframes AtFSnXJu_draw{100%{stroke-dashoffset:0;}}@keyframes AtFSnXJu_fade{0%{stroke-opacity:1;}93.22033898305085%{stroke-opacity:1;}100%{stroke-opacity:0;}}@keyframes AtFSnXJu_draw_0{13.559322033898304%{stroke-dashoffset: 2657}32.76836158192091%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_1{13.850368087656223%{stroke-dashoffset: 2657}33.05940763567883%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_2{14.14141414141414%{stroke-dashoffset: 2656}33.35045368943674%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_3{14.432460195172059%{stroke-dashoffset: 2657}33.64149974319466%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_4{14.723506248929978%{stroke-dashoffset: 2656}33.93254579695258%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_5{15.014552302687898%{stroke-dashoffset: 2657}34.2235918507105%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_6{15.305598356445813%{stroke-dashoffset: 2656}34.514637904468415%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_7{15.596644410203734%{stroke-dashoffset: 2657}34.80568395822633%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_8{15.88769046396165%{stroke-dashoffset: 2657}35.09673001198425%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_9{16.178736517719567%{stroke-dashoffset: 2657}35.38777606574217%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_10{16.469782571477488%{stroke-dashoffset: 2657}35.67882211950009%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_11{16.760828625235405%{stroke-dashoffset: 1267}35.969868173258014%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_12{17.05187467899332%{stroke-dashoffset: 1267}36.260914227015924%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_13{17.342920732751242%{stroke-dashoffset: 1265}36.55196028077384%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_14{17.63396678650916%{stroke-dashoffset: 1267}36.84300633453176%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_15{17.925012840267076%{stroke-dashoffset: 1267}37.13405238828968%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_16{18.216058894024997%{stroke-dashoffset: 1267}37.4250984420476%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_17{18.507104947782913%{stroke-dashoffset: 1265}37.71614449580552%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_18{18.79815100154083%{stroke-dashoffset: 1267}38.00719054956343%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_19{19.08919705529875%{stroke-dashoffset: 1267}38.29823660332135%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_20{19.380243109056668%{stroke-dashoffset: 1267}38.589282657079266%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_21{19.671289162814585%{stroke-dashoffset: 1267}38.88032871083718%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_22{19.962335216572505%{stroke-dashoffset: 1265}39.17137476459511%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_23{20.253381270330422%{stroke-dashoffset: 1267}39.462420818353024%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_24{20.54442732408834%{stroke-dashoffset: 1267}39.75346687211094%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_25{20.83547337784626%{stroke-dashoffset: 1267}40.044512925868865%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_26{21.126519431604176%{stroke-dashoffset: 1265}40.335558979626775%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_27{21.417565485362093%{stroke-dashoffset: 1267}40.62660503338469%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_28{21.708611539120014%{stroke-dashoffset: 1267}40.917651087142616%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_29{21.99965759287793%{stroke-dashoffset: 1267}41.20869714090053%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_30{22.290703646635848%{stroke-dashoffset: 1267}41.49974319465845%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_31{22.581749700393768%{stroke-dashoffset: 1265}41.790789248416374%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_32{22.872795754151685%{stroke-dashoffset: 1267}42.081835302174284%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}@keyframes AtFSnXJu_draw_33{23.163841807909602%{stroke-dashoffset: 3932}42.3728813559322%{ stroke-dashoffset: 0;}100%{ stroke-dashoffset: 0;}}</style></svg>
  
        </div>
  
      </div>
      <div class="grid-pt-02">
      <svg width="100%" height="100% " viewBox="0 0 1332 200" fill="none" xmlns="http://www.w3.org/2000/svg">
  <path d="M5.51256 0.142535V4.81312C5.51256 5.91655 5.30758 6.69888 4.89761 7.15936C4.48765 7.61984 3.78851 7.8501 2.80021 7.8501C1.77529 7.8501 1.05053 7.61984 0.625928 7.15936C0.208643 6.69888 0 5.91655 0 4.81312V0.142535H0.966346V4.81312C0.966346 5.62445 1.09812 6.19064 1.36167 6.5125C1.62522 6.82654 2.09009 6.98394 2.75628 6.98394C3.41515 6.98394 3.87636 6.82654 4.13991 6.5125C4.41078 6.19064 4.54622 5.62445 4.54622 4.81312V0.142535H5.51256ZM7.36854 7.70756V0.142535C7.98349 0.0767522 8.851 0.0438313 9.97109 0.0438313C10.8935 0.0438313 11.5304 0.21219 11.8818 0.548154C12.2332 0.877069 12.4089 1.4981 12.4089 2.41201C12.4089 3.32592 12.2369 3.9509 11.8928 4.28686C11.5561 4.61578 10.9521 4.78024 10.0809 4.78024C9.48062 4.78024 8.89861 4.74732 8.33489 4.68153V7.70756H7.36854ZM8.33489 3.94696H9.9601C10.5238 3.93992 10.9082 3.83341 11.1131 3.62901C11.3255 3.41678 11.4316 3.01111 11.4316 2.41201C11.4316 1.81291 11.3255 1.40729 11.1131 1.19506C10.9009 0.982831 10.5165 0.877103 9.9601 0.877103C9.16216 0.877103 8.6204 0.881001 8.33489 0.888049V3.94696ZM13.8457 7.70756V0.142535C14.7975 0.0767522 15.6979 0.0438313 16.5471 0.0438313C17.6379 0.0438313 18.4029 0.328923 18.8422 0.899043C19.2814 1.46211 19.501 2.47075 19.501 3.92502C19.501 5.3793 19.2814 6.39188 18.8422 6.962C18.4029 7.52507 17.6379 7.80622 16.5471 7.80622C15.6979 7.80622 14.7975 7.77335 13.8457 7.70756ZM14.8121 0.899043V6.94011C15.4709 6.95499 16.0493 6.962 16.5471 6.962C17.2719 6.962 17.777 6.74275 18.0625 6.3042C18.348 5.8586 18.4908 5.06526 18.4908 3.92502C18.4908 2.78479 18.348 1.9915 18.0625 1.5459C17.777 1.1003 17.2719 0.877103 16.5471 0.877103C16.0786 0.877103 15.5003 0.884163 14.8121 0.899043ZM25.1142 5.33938H21.9955L21.2488 7.70756H20.2495L22.6873 0.405666C22.7312 0.230245 22.852 0.142535 23.0497 0.142535H24.06C24.2576 0.142535 24.3784 0.230245 24.4223 0.405666L26.8602 7.70756H25.8609L25.1142 5.33938ZM24.8506 4.50611L23.8952 1.50202C23.8294 1.31172 23.7672 1.10733 23.7086 0.888049H23.4011L23.2144 1.50202L22.259 4.50611H24.8506ZM32.4439 0.997699H29.9951V7.70756H29.0287V0.997699H26.5909V0.142535H32.4439V0.997699ZM38.2645 6.90722L38.3085 7.65274C37.6349 7.69659 36.5515 7.71851 35.058 7.71851C34.6114 7.71851 34.2564 7.60182 33.9928 7.36767C33.7366 7.12646 33.6048 6.8046 33.5975 6.40285V1.4472C33.6048 1.04545 33.7366 0.727533 33.9928 0.493377C34.2564 0.252172 34.6114 0.131542 35.058 0.131542C36.5515 0.131542 37.6349 0.153457 38.3085 0.197312L38.2645 0.95382H35.2227C34.7835 0.95382 34.5639 1.19503 34.5639 1.67744V3.32205H37.8363V4.10045H34.5639V6.17261C34.5639 6.66206 34.7835 6.90722 35.2227 6.90722H38.2645ZM39.8081 7.70756V0.142535C40.7599 0.0767522 41.6603 0.0438313 42.5095 0.0438313C43.6003 0.0438313 44.3653 0.328923 44.8045 0.899043C45.2438 1.46211 45.4634 2.47075 45.4634 3.92502C45.4634 5.3793 45.2438 6.39188 44.8045 6.962C44.3653 7.52507 43.6003 7.80622 42.5095 7.80622C41.6603 7.80622 40.7599 7.77335 39.8081 7.70756ZM40.7744 0.899043V6.94011C41.4333 6.95499 42.0117 6.962 42.5095 6.962C43.2342 6.962 43.7394 6.74275 44.0249 6.3042C44.3104 5.8586 44.4531 5.06526 44.4531 3.92502C44.4531 2.78479 44.3104 1.9915 44.0249 1.5459C43.7394 1.1003 43.2342 0.877103 42.5095 0.877103C42.041 0.877103 41.4627 0.884163 40.7744 0.899043ZM53.2964 5.33938H50.1777L49.431 7.70756H48.4317L50.8695 0.405666C50.9135 0.230245 51.0343 0.142535 51.2319 0.142535H52.2422C52.4398 0.142535 52.5606 0.230245 52.6046 0.405666L55.0424 7.70756H54.0431L53.2964 5.33938ZM53.0328 4.50611L52.0775 1.50202C52.0116 1.31172 51.9494 1.10733 51.8908 0.888049H51.5833L51.3966 1.50202L50.4413 4.50611H53.0328ZM58.8207 4.50611L57.2504 4.11144C56.716 3.97987 56.3317 3.74572 56.0974 3.40976C55.8705 3.06596 55.757 2.59839 55.757 2.00634C55.757 1.56779 55.8009 1.21695 55.8887 0.95382C55.9839 0.68364 56.1413 0.482407 56.3609 0.350841C56.5878 0.212227 56.8368 0.120572 57.1076 0.0767167C57.3859 0.0328613 57.7555 0.0109459 58.2167 0.0109459C59.1099 0.0258254 59.8602 0.0986795 60.4679 0.230246L60.402 0.931928C59.6919 0.895121 58.9781 0.877103 58.2607 0.877103C57.9385 0.877103 57.7006 0.884163 57.5469 0.899043C57.3932 0.913922 57.2431 0.957753 57.0967 1.03058C56.9502 1.10342 56.8514 1.22088 56.8002 1.38143C56.7562 1.53492 56.7343 1.75027 56.7343 2.02828C56.7343 2.43003 56.8039 2.71512 56.9429 2.88349C57.0894 3.05187 57.3382 3.17951 57.6896 3.26722L59.227 3.65095C59.7907 3.78956 60.1861 4.02761 60.413 4.36358C60.6399 4.69954 60.7534 5.17101 60.7534 5.77794C60.7534 6.61119 60.563 7.16331 60.1824 7.43349C59.809 7.70367 59.1538 7.83915 58.2167 7.83915C57.4188 7.83915 56.6355 7.76941 55.8668 7.6308L55.9327 6.91817C57.1625 6.96202 57.9349 6.98004 58.2497 6.97299C58.85 6.97299 59.2527 6.89624 59.4576 6.74275C59.6699 6.58926 59.7761 6.26033 59.7761 5.756C59.7761 5.33937 59.7029 5.05038 59.5564 4.88984C59.4174 4.72147 59.1721 4.59382 58.8207 4.50611ZM65.0822 0.86611C65.5288 0.288942 66.3085 0 67.4212 0C68.534 0 69.31 0.288942 69.7492 0.86611C70.1884 1.43623 70.4081 2.45587 70.4081 3.92502C70.4081 5.39418 70.1884 6.41777 69.7492 6.99493C69.31 7.56505 68.534 7.8501 67.4212 7.8501C66.3085 7.8501 65.5288 7.56505 65.0822 6.99493C64.6429 6.41777 64.4233 5.39418 64.4233 3.92502C64.4233 2.45587 64.6429 1.43623 65.0822 0.86611ZM68.9695 1.53491C68.684 1.08931 68.1679 0.86611 67.4212 0.86611C66.6745 0.86611 66.1583 1.08931 65.8728 1.53491C65.5873 1.97346 65.4446 2.76991 65.4446 3.92502C65.4446 5.08014 65.5873 5.88054 65.8728 6.32614C66.1583 6.76469 66.6745 6.98394 67.4212 6.98394C68.1679 6.98394 68.684 6.76469 68.9695 6.32614C69.255 5.88054 69.3978 5.08014 69.3978 3.92502C69.3978 2.76991 69.255 1.97346 68.9695 1.53491ZM76.5682 0.95382H73.7241C73.4678 0.95382 73.2811 1.01255 73.164 1.12924C73.0469 1.23888 72.9883 1.42136 72.9883 1.67744V3.43165H76.1509V4.2101H72.9883V7.70756H72.022V1.49108C72.022 1.05957 72.1538 0.727533 72.4173 0.493377C72.6809 0.252172 73.0359 0.131542 73.4825 0.131542C74.9247 0.131542 75.9715 0.157403 76.6231 0.208306L76.5682 0.95382ZM80.5667 0.909989C80.9542 0.303062 81.628 0 82.5872 0C83.5457 0 84.2195 0.303062 84.6078 0.909989C85.0031 1.51692 85.2008 2.52165 85.2008 3.92502C85.2008 5.3284 85.0031 6.33318 84.6078 6.94011C84.2195 7.54704 83.5457 7.8501 82.5872 7.8501C81.628 7.8501 80.9542 7.54704 80.5667 6.94011C80.1784 6.33318 79.9847 5.3284 79.9847 3.92502C79.9847 2.52165 80.1784 1.51692 80.5667 0.909989ZM83.883 1.53491C83.6414 1.07443 83.2092 0.844218 82.5872 0.844218C81.9645 0.844218 81.533 1.07443 81.2915 1.53491C81.0499 1.99539 80.9291 2.79183 80.9291 3.92502C80.9291 5.05822 81.0499 5.85466 81.2915 6.31514C81.533 6.77562 81.9645 7.00588 82.5872 7.00588C83.2092 7.00588 83.6414 6.77562 83.883 6.31514C84.1317 5.85466 84.2564 5.05822 84.2564 3.92502C84.2564 2.79183 84.1317 1.99539 83.883 1.53491ZM87.9076 3.80443V3.7606C87.1829 3.67994 86.8205 3.10274 86.8205 2.02828C86.8205 1.31172 86.9891 0.800364 87.3256 0.493377C87.67 0.186389 88.3069 0.0328854 89.2364 0.0328854C90.1658 0.0328854 90.7996 0.186389 91.1361 0.493377C91.4726 0.793316 91.6412 1.30467 91.6412 2.02828C91.6412 3.10274 91.2828 3.67994 90.5651 3.7606V3.80443C91.0192 3.84828 91.3377 4.03861 91.5204 4.37457C91.711 4.71053 91.806 5.19291 91.806 5.82177C91.806 6.53833 91.6263 7.04973 91.2679 7.35672C90.9165 7.65666 90.2396 7.81016 89.2364 7.81721C88.248 7.82426 87.5711 7.67465 87.2048 7.36767C86.8385 7.06068 86.6558 6.54538 86.6558 5.82177C86.6558 5.19291 86.7507 4.71053 86.9413 4.37457C87.1319 4.03861 87.4535 3.84828 87.9076 3.80443ZM90.4662 1.08541C90.2686 0.90294 89.8623 0.811285 89.2473 0.811285C88.6324 0.811285 88.2222 0.90294 88.0174 1.08541C87.8127 1.26788 87.71 1.61166 87.71 2.11599C87.71 2.63521 87.8088 2.97825 88.0065 3.14663C88.2112 3.315 88.6285 3.39876 89.2583 3.39876C89.8882 3.39876 90.2945 3.315 90.4772 3.14663C90.6678 2.97825 90.7627 2.63521 90.7627 2.11599C90.7627 1.61166 90.6639 1.26788 90.4662 1.08541ZM89.2583 4.16626C88.5704 4.17331 88.1163 4.27198 87.8966 4.46228C87.677 4.65258 87.5672 5.03946 87.5672 5.62445C87.5672 6.14367 87.6809 6.50152 87.9076 6.69887C88.1343 6.88917 88.5814 6.98394 89.2473 6.98394C89.8991 6.98394 90.3345 6.88917 90.5541 6.69887C90.7737 6.50857 90.8835 6.15072 90.8835 5.62445C90.8835 5.02536 90.7776 4.63376 90.5651 4.45129C90.3604 4.26099 89.9242 4.16626 89.2583 4.16626ZM96.7083 5.2955H93.359V4.58288H96.7083V5.2955ZM101.04 4.50611L99.47 4.11144C98.9359 3.97987 98.5515 3.74572 98.317 3.40976C98.0903 3.06596 97.9766 2.59839 97.9766 2.00634C97.9766 1.56779 98.0205 1.21695 98.1084 0.95382C98.2033 0.68364 98.361 0.482407 98.5806 0.350841C98.8073 0.212227 99.0567 0.120572 99.3273 0.0767167C99.6057 0.0328613 99.9752 0.0109459 100.436 0.0109459C101.33 0.0258254 102.08 0.0986795 102.688 0.230246L102.622 0.931928C101.912 0.895121 101.198 0.877103 100.48 0.877103C100.158 0.877103 99.9203 0.884163 99.7665 0.899043C99.6128 0.913922 99.463 0.957753 99.3163 1.03058C99.1696 1.10342 99.0708 1.22088 99.0198 1.38143C98.9759 1.53492 98.9539 1.75027 98.9539 2.02828C98.9539 2.43003 99.0237 2.71512 99.1626 2.88349C99.3093 3.05187 99.5579 3.17951 99.9093 3.26722L101.447 3.65095C102.011 3.78956 102.406 4.02761 102.633 4.36358C102.859 4.69954 102.973 5.17101 102.973 5.77794C102.973 6.61119 102.782 7.16331 102.402 7.43349C102.029 7.70367 101.374 7.83915 100.436 7.83915C99.6387 7.83915 98.8551 7.76941 98.0864 7.6308L98.1523 6.91817C99.3822 6.96202 100.155 6.98004 100.469 6.97299C101.069 6.97299 101.473 6.89624 101.677 6.74275C101.89 6.58926 101.996 6.26033 101.996 5.756C101.996 5.33937 101.923 5.05038 101.776 4.88984C101.637 4.72147 101.392 4.59382 101.04 4.50611ZM109.085 6.90722L109.129 7.65274C108.455 7.69659 107.372 7.71851 105.878 7.71851C105.431 7.71851 105.077 7.60182 104.813 7.36767C104.557 7.12646 104.425 6.8046 104.418 6.40285V1.4472C104.425 1.04545 104.557 0.727533 104.813 0.493377C105.077 0.252172 105.431 0.131542 105.878 0.131542C107.372 0.131542 108.455 0.153457 109.129 0.197312L109.085 0.95382H106.043C105.604 0.95382 105.384 1.19503 105.384 1.67744V3.32205H108.657V4.10045H105.384V6.17261C105.384 6.66206 105.604 6.90722 106.043 6.90722H109.085ZM110.629 7.70756V0.142535C111.243 0.0767522 112.111 0.0438313 113.231 0.0438313C114.153 0.0438313 114.79 0.21219 115.142 0.548154C115.493 0.877069 115.669 1.4981 115.669 2.41201C115.669 3.32592 115.496 3.9509 115.153 4.28686C114.815 4.61578 114.212 4.78024 113.341 4.78024C112.74 4.78024 112.158 4.74732 111.595 4.68153V7.70756H110.629ZM111.595 3.94696H113.22C113.783 3.93992 114.168 3.83341 114.373 3.62901C114.585 3.41678 114.692 3.01111 114.692 2.41201C114.692 1.81291 114.585 1.40729 114.373 1.19506C114.161 0.982831 113.776 0.877103 113.22 0.877103C112.422 0.877103 111.88 0.881001 111.595 0.888049V3.94696ZM120.236 5.2955H116.887V4.58288H120.236V5.2955ZM121.765 1.04153L121.699 0.296016C122.512 0.105715 123.28 0.0109459 124.005 0.0109459C124.804 0.0109459 125.389 0.124516 125.762 0.350841C126.143 0.570118 126.333 1.00161 126.333 1.64456C126.333 2.11992 126.234 2.55847 126.037 2.96021C125.839 3.35491 125.488 3.83339 124.983 4.39646L122.808 6.81951C123.153 6.79054 123.563 6.77563 124.038 6.77563H126.608V7.70756H121.633V7.10454C121.633 6.92911 121.681 6.79051 121.776 6.68792L123.972 4.2101C124.47 3.67678 124.829 3.21237 125.048 2.81768C125.268 2.41593 125.371 2.03928 125.356 1.68844C125.349 1.34464 125.232 1.12223 125.004 1.01964C124.785 0.917048 124.43 0.86611 123.939 0.86611C123.207 0.86611 122.483 0.924844 121.765 1.04153ZM128.481 0.909989C128.868 0.303062 129.542 0 130.501 0C131.46 0 132.134 0.303062 132.522 0.909989C132.917 1.51692 133.115 2.52165 133.115 3.92502C133.115 5.3284 132.917 6.33318 132.522 6.94011C132.134 7.54704 131.46 7.8501 130.501 7.8501C129.542 7.8501 128.868 7.54704 128.481 6.94011C128.093 6.33318 127.899 5.3284 127.899 3.92502C127.899 2.52165 128.093 1.51692 128.481 0.909989ZM131.797 1.53491C131.556 1.07443 131.123 0.844218 130.501 0.844218C129.879 0.844218 129.447 1.07443 129.206 1.53491C128.964 1.99539 128.843 2.79183 128.843 3.92502C128.843 5.05822 128.964 5.85466 129.206 6.31514C129.447 6.77562 129.879 7.00588 130.501 7.00588C131.123 7.00588 131.556 6.77562 131.797 6.31514C132.046 5.85466 132.17 5.05822 132.17 3.92502C132.17 2.79183 132.046 1.99539 131.797 1.53491ZM134.516 1.04153L134.45 0.296016C135.262 0.105715 136.031 0.0109459 136.756 0.0109459C137.554 0.0109459 138.14 0.124516 138.513 0.350841C138.893 0.570118 139.084 1.00161 139.084 1.64456C139.084 2.11992 138.985 2.55847 138.787 2.96021C138.59 3.35491 138.238 3.83339 137.733 4.39646L135.559 6.81951C135.903 6.79054 136.313 6.77563 136.789 6.77563H139.358V7.70756H134.384V7.10454C134.384 6.92911 134.432 6.79051 134.527 6.68792L136.723 4.2101C137.221 3.67678 137.58 3.21237 137.799 2.81768C138.019 2.41593 138.122 2.03928 138.107 1.68844C138.1 1.34464 137.982 1.12223 137.755 1.01964C137.536 0.917048 137.18 0.86611 136.69 0.86611C135.958 0.86611 135.233 0.924844 134.516 1.04153ZM143.087 7.70756H142.12V1.71033C142.12 1.45424 142.139 1.23499 142.175 1.05252L140.319 1.63361L140.188 0.920934L142.12 0.142535H143.087V7.70756Z" fill="#F5F5F0"/>
  <path d="M20.278 41.3821H1.45312V53.9122H20.278V41.3821Z" fill="#D3D3D3"/>
  <path d="M220.292 41.3821H114.402V53.9122H220.292V41.3821Z" fill="#D3D3D3"/>
  <path d="M323.829 41.3821H314.417V53.9122H323.829V41.3821Z" fill="#D3D3D3"/>
  <path d="M640.557 54.9149V46.9081H648.576V54.9149H640.557ZM644.82 54.4074H648.068V47.4155H641.065V50.6577H644.82V54.4074ZM641.065 54.4074H644.312V51.1652H641.065V54.4074ZM658.879 55.4787L653.189 45.6535H664.554L658.879 55.4787ZM673.057 55.6056L673.17 51.2639L668.821 51.3767V49.4596L673.17 49.6005L673.057 45.2588H674.977L674.864 49.6005L679.185 49.4596V51.3767L674.864 51.2639L674.977 55.6056H673.057ZM673.255 51.2075H674.779V49.6569H673.255V51.2075ZM683.448 55.4787V45.9495H692.992V55.4787H683.448ZM684.125 54.8021H692.314V46.6261H684.125V54.8021ZM688.22 51.5599C687.994 51.5599 687.796 51.48 687.627 51.3203C687.457 51.1511 687.373 50.9491 687.373 50.7141C687.373 50.4792 687.453 50.2818 687.613 50.122C687.782 49.9529 687.984 49.8683 688.22 49.8683C688.464 49.8683 688.667 49.9576 688.827 50.1361C688.987 50.3053 689.067 50.498 689.067 50.7141C689.067 50.9491 688.982 51.1511 688.813 51.3203C688.653 51.48 688.455 51.5599 688.22 51.5599Z" fill="white"/>
  <path d="M1018.78 41.3821H1009.37V53.9122H1018.78V41.3821Z" fill="#D3D3D3"/>
  <path d="M1218.8 41.3821H1112.91V53.9122H1218.8V41.3821Z" fill="#D3D3D3"/>
  <path d="M1331.75 41.3821H1312.92V53.9122H1331.75V41.3821Z" fill="#D3D3D3"/>
  <path d="M687.386 86.4121H645.814V127.918H687.386V86.4121Z" fill="white"/>
  <path fill-rule="evenodd" clip-rule="evenodd" d="M645.314 85.9121H687.886V128.418H645.314V85.9121ZM646.314 86.9121V127.418H686.886V86.9121H646.314Z" fill="white"/>
  <path d="M554.099 199.575C554.026 199.575 553.989 199.536 553.989 199.465V192.01C553.989 191.939 554.026 191.9 554.099 191.9H556.691C557.415 191.9 557.99 192.088 558.415 192.472C558.847 192.848 559.063 193.365 559.063 194.015V197.46C559.063 198.11 558.847 198.627 558.415 199.003C557.99 199.387 557.415 199.575 556.691 199.575H554.099ZM554.769 198.854C554.769 198.878 554.784 198.894 554.813 198.894H556.724C557.2 198.894 557.577 198.753 557.855 198.478C558.14 198.204 558.283 197.828 558.283 197.359V194.124C558.283 193.654 558.144 193.271 557.866 192.997C557.588 192.722 557.207 192.581 556.724 192.581H554.813C554.784 192.581 554.769 192.597 554.769 192.621V198.854ZM567.949 192.472C567.949 192.542 567.912 192.581 567.839 192.581H563.798C563.768 192.581 563.754 192.597 563.754 192.621V195.33C563.754 195.362 563.768 195.377 563.798 195.377H566.631C566.704 195.377 566.741 195.409 566.741 195.487V195.949C566.741 196.019 566.704 196.059 566.631 196.059H563.798C563.768 196.059 563.754 196.066 563.754 196.098V198.854C563.754 198.878 563.768 198.894 563.798 198.894H567.839C567.912 198.894 567.949 198.933 567.949 199.003V199.465C567.949 199.536 567.912 199.575 567.839 199.575H563.084C563.01 199.575 562.974 199.536 562.974 199.465V192.01C562.974 191.939 563.01 191.9 563.084 191.9H567.839C567.912 191.9 567.949 191.939 567.949 192.01V192.472ZM576.376 199.575C576.31 199.575 576.266 199.544 576.244 199.489L575.805 198.103C575.798 198.087 575.783 198.071 575.761 198.071H572.478C572.456 198.071 572.442 198.087 572.434 198.103L571.995 199.489C571.973 199.544 571.929 199.575 571.863 199.575H571.27C571.19 199.575 571.16 199.536 571.182 199.457L573.609 191.986C573.631 191.932 573.675 191.9 573.74 191.9H574.487C574.553 191.9 574.597 191.932 574.619 191.986L577.057 199.457L577.068 199.497C577.068 199.551 577.035 199.575 576.969 199.575H576.376ZM572.653 197.382C572.646 197.398 572.646 197.413 572.653 197.429C572.668 197.437 572.683 197.437 572.697 197.437H575.53C575.545 197.437 575.556 197.437 575.563 197.429C575.578 197.413 575.582 197.398 575.574 197.382L574.147 192.91C574.14 192.895 574.129 192.887 574.114 192.887C574.1 192.887 574.089 192.895 574.081 192.91L572.653 197.382ZM580.795 199.575C580.722 199.575 580.685 199.536 580.685 199.465V192.01C580.685 191.939 580.722 191.9 580.795 191.9H581.355C581.429 191.9 581.465 191.939 581.465 192.01V198.854C581.465 198.878 581.48 198.894 581.509 198.894H585.385C585.459 198.894 585.495 198.933 585.495 199.003V199.465C585.495 199.536 585.459 199.575 585.385 199.575H580.795ZM594.097 192.472C594.097 192.542 594.061 192.581 593.988 192.581H589.946C589.917 192.581 589.903 192.597 589.903 192.621V195.33C589.903 195.362 589.917 195.377 589.946 195.377H592.78C592.853 195.377 592.889 195.409 592.889 195.487V195.949C592.889 196.019 592.853 196.059 592.78 196.059H589.946C589.917 196.059 589.903 196.066 589.903 196.098V198.854C589.903 198.878 589.917 198.894 589.946 198.894H593.988C594.061 198.894 594.097 198.933 594.097 199.003V199.465C594.097 199.536 594.061 199.575 593.988 199.575H589.233C589.16 199.575 589.123 199.536 589.123 199.465V192.01C589.123 191.939 589.16 191.9 589.233 191.9H593.988C594.061 191.9 594.097 191.939 594.097 192.01V192.472ZM602.449 199.575C602.383 199.575 602.339 199.551 602.317 199.497L600.703 196.098C600.695 196.074 600.681 196.066 600.659 196.066H598.792C598.762 196.066 598.748 196.082 598.748 196.113V199.465C598.748 199.536 598.711 199.575 598.638 199.575H598.078C598.004 199.575 597.968 199.536 597.968 199.465V192.01C597.968 191.939 598.004 191.9 598.078 191.9H600.922C601.551 191.9 602.06 192.096 602.449 192.495C602.836 192.879 603.031 193.388 603.031 194.015C603.031 194.531 602.891 194.962 602.613 195.322C602.335 195.683 601.958 195.91 601.482 196.012C601.452 196.027 601.445 196.043 601.46 196.066L603.107 199.442C603.114 199.457 603.118 199.473 603.118 199.497C603.118 199.551 603.089 199.575 603.031 199.575H602.449ZM598.792 192.581C598.762 192.581 598.748 192.597 598.748 192.621V195.401C598.748 195.424 598.762 195.44 598.792 195.44H600.834C601.259 195.44 601.603 195.307 601.867 195.048C602.13 194.782 602.262 194.438 602.262 194.015C602.262 193.592 602.13 193.247 601.867 192.989C601.603 192.715 601.259 192.581 600.834 192.581H598.792ZM610.701 199.575C610.62 199.575 610.591 199.536 610.613 199.457L613.04 191.986C613.062 191.932 613.106 191.9 613.172 191.9H613.721C613.801 191.9 613.831 191.939 613.809 192.018L611.382 199.489C611.36 199.544 611.316 199.575 611.25 199.575H610.701ZM627.256 191.978C627.293 191.924 627.337 191.9 627.388 191.9H627.959C628.032 191.9 628.068 191.939 628.068 192.01V199.465C628.068 199.536 628.032 199.575 627.959 199.575H627.399C627.326 199.575 627.289 199.536 627.289 199.465V193.372C627.289 193.349 627.282 193.333 627.267 193.325C627.252 193.318 627.241 193.325 627.234 193.349L625.444 196.09C625.407 196.137 625.363 196.168 625.312 196.168H625.027C624.976 196.168 624.932 196.145 624.895 196.098L623.105 193.38C623.098 193.357 623.087 193.349 623.072 193.357C623.057 193.365 623.05 193.38 623.05 193.404V199.465C623.05 199.536 623.013 199.575 622.94 199.575H622.38C622.307 199.575 622.27 199.536 622.27 199.465V192.01C622.27 191.939 622.307 191.9 622.38 191.9H622.951C623.01 191.9 623.054 191.924 623.083 191.963L625.147 195.087C625.154 195.095 625.165 195.103 625.18 195.103C625.195 195.103 625.206 195.095 625.213 195.087L627.256 191.978ZM636.874 199.575C636.808 199.575 636.764 199.544 636.742 199.489L636.303 198.103C636.295 198.087 636.281 198.071 636.259 198.071H632.975C632.954 198.071 632.939 198.087 632.932 198.103L632.492 199.489C632.47 199.544 632.426 199.575 632.361 199.575H631.768C631.687 199.575 631.658 199.536 631.68 199.457L634.107 191.986C634.128 191.932 634.172 191.9 634.238 191.9H634.985C635.051 191.9 635.095 191.932 635.117 191.986L637.555 199.457L637.566 199.497C637.566 199.551 637.533 199.575 637.467 199.575H636.874ZM633.151 197.382C633.143 197.398 633.143 197.413 633.151 197.429C633.165 197.437 633.18 197.437 633.195 197.437H636.028C636.042 197.437 636.053 197.437 636.061 197.429C636.075 197.413 636.079 197.398 636.072 197.382L634.645 192.91C634.637 192.895 634.626 192.887 634.612 192.887C634.597 192.887 634.586 192.895 634.579 192.91L633.151 197.382ZM643.394 199.677C642.881 199.677 642.43 199.575 642.043 199.379C641.655 199.175 641.351 198.886 641.131 198.525C640.919 198.15 640.813 197.719 640.813 197.241V194.226C640.813 193.748 640.919 193.325 641.131 192.965C641.351 192.597 641.655 192.315 642.043 192.119C642.43 191.916 642.881 191.814 643.394 191.814C643.906 191.814 644.356 191.908 644.744 192.112C645.132 192.307 645.432 192.581 645.645 192.942C645.864 193.302 645.974 193.709 645.974 194.179C645.974 194.218 645.963 194.25 645.941 194.265C645.919 194.289 645.893 194.304 645.864 194.304L645.304 194.336C645.23 194.336 645.194 194.304 645.194 194.234V194.203C645.194 193.686 645.03 193.271 644.7 192.965C644.371 192.652 643.935 192.495 643.394 192.495C642.852 192.495 642.416 192.652 642.087 192.965C641.757 193.278 641.593 193.694 641.593 194.203V197.272C641.593 197.781 641.757 198.197 642.087 198.51C642.416 198.823 642.852 198.98 643.394 198.98C643.935 198.98 644.371 198.831 644.7 198.525C645.03 198.204 645.194 197.789 645.194 197.272V197.249C645.194 197.186 645.23 197.155 645.304 197.155L645.864 197.186C645.937 197.186 645.974 197.218 645.974 197.28C645.974 197.758 645.864 198.181 645.645 198.541C645.432 198.901 645.132 199.183 644.744 199.379C644.356 199.575 643.906 199.677 643.394 199.677ZM654.292 192.01C654.292 191.939 654.328 191.9 654.401 191.9H654.961C655.034 191.9 655.071 191.939 655.071 192.01V199.465C655.071 199.536 655.034 199.575 654.961 199.575H654.401C654.328 199.575 654.292 199.536 654.292 199.465V196.098C654.292 196.066 654.277 196.059 654.248 196.059H650.745C650.715 196.059 650.701 196.066 650.701 196.098V199.465C650.701 199.536 650.664 199.575 650.591 199.575H650.031C649.957 199.575 649.921 199.536 649.921 199.465V192.01C649.921 191.939 649.957 191.9 650.031 191.9H650.591C650.664 191.9 650.701 191.939 650.701 192.01V195.33C650.701 195.362 650.715 195.377 650.745 195.377H654.248C654.277 195.377 654.292 195.362 654.292 195.33V192.01ZM659.412 199.575C659.338 199.575 659.302 199.536 659.302 199.465V192.01C659.302 191.939 659.338 191.9 659.412 191.9H659.972C660.045 191.9 660.082 191.939 660.082 192.01V199.465C660.082 199.536 660.045 199.575 659.972 199.575H659.412ZM668.919 192.01C668.919 191.939 668.955 191.9 669.029 191.9H669.589C669.662 191.9 669.699 191.939 669.699 192.01V199.465C669.699 199.536 669.662 199.575 669.589 199.575H669.04C668.988 199.575 668.944 199.551 668.908 199.497L665.164 193.412C665.156 193.388 665.145 193.38 665.131 193.388C665.116 193.388 665.109 193.404 665.109 193.427L665.12 199.465C665.12 199.536 665.083 199.575 665.01 199.575H664.45C664.376 199.575 664.34 199.536 664.34 199.465V192.01C664.34 191.939 664.376 191.9 664.45 191.9H664.999C665.05 191.9 665.094 191.924 665.131 191.978L668.875 198.063C668.882 198.087 668.893 198.095 668.908 198.095C668.922 198.087 668.93 198.071 668.93 198.048L668.919 192.01ZM678.781 192.472C678.781 192.542 678.745 192.581 678.671 192.581H674.63C674.601 192.581 674.586 192.597 674.586 192.621V195.33C674.586 195.362 674.601 195.377 674.63 195.377H677.463C677.537 195.377 677.573 195.409 677.573 195.487V195.949C677.573 196.019 677.537 196.059 677.463 196.059H674.63C674.601 196.059 674.586 196.066 674.586 196.098V198.854C674.586 198.878 674.601 198.894 674.63 198.894H678.671C678.745 198.894 678.781 198.933 678.781 199.003V199.465C678.781 199.536 678.745 199.575 678.671 199.575H673.917C673.844 199.575 673.807 199.536 673.807 199.465V192.01C673.807 191.939 673.844 191.9 673.917 191.9H678.671C678.745 191.9 678.781 191.939 678.781 192.01V192.472ZM687.132 199.575C687.067 199.575 687.023 199.551 687.001 199.497L685.386 196.098C685.379 196.074 685.364 196.066 685.342 196.066H683.476C683.447 196.066 683.432 196.082 683.432 196.113V199.465C683.432 199.536 683.395 199.575 683.322 199.575H682.762C682.689 199.575 682.652 199.536 682.652 199.465V192.01C682.652 191.939 682.689 191.9 682.762 191.9H685.606C686.236 191.9 686.744 192.096 687.132 192.495C687.521 192.879 687.714 193.388 687.714 194.015C687.714 194.531 687.576 194.962 687.297 195.322C687.019 195.683 686.642 195.91 686.166 196.012C686.137 196.027 686.129 196.043 686.144 196.066L687.791 199.442C687.798 199.457 687.802 199.473 687.802 199.497C687.802 199.551 687.773 199.575 687.714 199.575H687.132ZM683.476 192.581C683.447 192.581 683.432 192.597 683.432 192.621V195.401C683.432 195.424 683.447 195.44 683.476 195.44H685.518C685.943 195.44 686.287 195.307 686.55 195.048C686.814 194.782 686.946 194.438 686.946 194.015C686.946 193.592 686.814 193.247 686.55 192.989C686.287 192.715 685.943 192.581 685.518 192.581H683.476ZM693.244 199.567C693.171 199.567 693.134 199.528 693.134 199.457V196.207C693.134 196.176 693.131 196.16 693.123 196.153L690.839 192.033C690.824 192.002 690.817 191.978 690.817 191.963C690.817 191.924 690.85 191.9 690.916 191.9H691.509C691.575 191.9 691.619 191.924 691.641 191.978L693.486 195.322C693.493 195.33 693.504 195.33 693.519 195.33C693.534 195.33 693.545 195.33 693.552 195.322L695.408 191.978C695.437 191.924 695.48 191.9 695.539 191.9H696.132C696.176 191.9 696.205 191.916 696.22 191.947C696.235 191.963 696.231 191.994 696.209 192.033L693.925 196.153C693.918 196.16 693.914 196.176 693.914 196.207V199.457C693.914 199.528 693.877 199.567 693.804 199.567H693.244ZM703.683 199.575C703.602 199.575 703.573 199.536 703.595 199.457L706.022 191.986C706.044 191.932 706.088 191.9 706.153 191.9H706.702C706.783 191.9 706.812 191.939 706.79 192.018L704.364 199.489C704.342 199.544 704.298 199.575 704.232 199.575H703.683ZM719.886 191.9C719.96 191.9 719.996 191.939 719.996 192.01V192.48C719.996 192.558 719.96 192.589 719.886 192.589H717.734C717.705 192.589 717.69 192.605 717.69 192.636V199.465C717.69 199.536 717.654 199.575 717.58 199.575H717.02C716.947 199.575 716.91 199.536 716.91 199.465V192.636C716.91 192.605 716.896 192.589 716.866 192.589H714.802C714.729 192.589 714.692 192.558 714.692 192.48V192.01C714.692 191.939 714.729 191.9 714.802 191.9H719.886ZM728.675 192.472C728.675 192.542 728.638 192.581 728.565 192.581H724.524C724.495 192.581 724.48 192.597 724.48 192.621V195.33C724.48 195.362 724.495 195.377 724.524 195.377H727.357C727.43 195.377 727.467 195.409 727.467 195.487V195.949C727.467 196.019 727.43 196.059 727.357 196.059H724.524C724.495 196.059 724.48 196.066 724.48 196.098V198.854C724.48 198.878 724.495 198.894 724.524 198.894H728.565C728.638 198.894 728.675 198.933 728.675 199.003V199.465C728.675 199.536 728.638 199.575 728.565 199.575H723.81C723.737 199.575 723.701 199.536 723.701 199.465V192.01C723.701 191.939 723.737 191.9 723.81 191.9H728.565C728.638 191.9 728.675 191.939 728.675 192.01V192.472ZM737.026 199.575C736.96 199.575 736.916 199.551 736.895 199.497L735.28 196.098C735.272 196.074 735.258 196.066 735.236 196.066H733.37C733.34 196.066 733.326 196.082 733.326 196.113V199.465C733.326 199.536 733.289 199.575 733.216 199.575H732.656C732.582 199.575 732.546 199.536 732.546 199.465V192.01C732.546 191.939 732.582 191.9 732.656 191.9H735.5C736.129 191.9 736.638 192.096 737.026 192.495C737.414 192.879 737.608 193.388 737.608 194.015C737.608 194.531 737.469 194.962 737.191 195.322C736.913 195.683 736.535 195.91 736.06 196.012C736.03 196.027 736.023 196.043 736.038 196.066L737.685 199.442C737.692 199.457 737.696 199.473 737.696 199.497C737.696 199.551 737.666 199.575 737.608 199.575H737.026ZM733.37 192.581C733.34 192.581 733.326 192.597 733.326 192.621V195.401C733.326 195.424 733.34 195.44 733.37 195.44H735.412C735.836 195.44 736.181 195.307 736.444 195.048C736.708 194.782 736.84 194.438 736.84 194.015C736.84 193.592 736.708 193.247 736.444 192.989C736.181 192.715 735.836 192.581 735.412 192.581H733.37ZM746.451 191.978C746.488 191.924 746.532 191.9 746.583 191.9H747.154C747.227 191.9 747.264 191.939 747.264 192.01V199.465C747.264 199.536 747.227 199.575 747.154 199.575H746.594C746.521 199.575 746.484 199.536 746.484 199.465V193.372C746.484 193.349 746.477 193.333 746.462 193.325C746.447 193.318 746.436 193.325 746.429 193.349L744.639 196.09C744.603 196.137 744.559 196.168 744.508 196.168H744.222C744.171 196.168 744.127 196.145 744.09 196.098L742.3 193.38C742.293 193.357 742.282 193.349 742.267 193.357C742.253 193.365 742.245 193.38 742.245 193.404V199.465C742.245 199.536 742.209 199.575 742.136 199.575H741.576C741.503 199.575 741.466 199.536 741.466 199.465V192.01C741.466 191.939 741.503 191.9 741.576 191.9H742.147C742.205 191.9 742.249 191.924 742.278 191.963L744.343 195.087C744.35 195.095 744.361 195.103 744.376 195.103C744.391 195.103 744.402 195.095 744.409 195.087L746.451 191.978ZM751.622 199.575C751.548 199.575 751.512 199.536 751.512 199.465V192.01C751.512 191.939 751.548 191.9 751.622 191.9H752.182C752.255 191.9 752.292 191.939 752.292 192.01V199.465C752.292 199.536 752.255 199.575 752.182 199.575H751.622ZM761.129 192.01C761.129 191.939 761.165 191.9 761.239 191.9H761.799C761.872 191.9 761.909 191.939 761.909 192.01V199.465C761.909 199.536 761.872 199.575 761.799 199.575H761.25C761.198 199.575 761.154 199.551 761.118 199.497L757.374 193.412C757.366 193.388 757.355 193.38 757.341 193.388C757.326 193.388 757.319 193.404 757.319 193.427L757.33 199.465C757.33 199.536 757.293 199.575 757.22 199.575H756.66C756.586 199.575 756.55 199.536 756.55 199.465V192.01C756.55 191.939 756.586 191.9 756.66 191.9H757.209C757.26 191.9 757.304 191.924 757.341 191.978L761.085 198.063C761.092 198.087 761.103 198.095 761.118 198.095C761.132 198.087 761.14 198.071 761.14 198.048L761.129 192.01ZM770.724 199.575C770.659 199.575 770.615 199.544 770.593 199.489L770.153 198.103C770.146 198.087 770.132 198.071 770.11 198.071H766.826C766.804 198.071 766.789 198.087 766.782 198.103L766.343 199.489C766.321 199.544 766.277 199.575 766.211 199.575H765.618C765.537 199.575 765.508 199.536 765.53 199.457L767.957 191.986C767.979 191.932 768.023 191.9 768.089 191.9H768.836C768.902 191.9 768.945 191.932 768.967 191.986L771.405 199.457L771.416 199.497C771.416 199.551 771.383 199.575 771.317 199.575H770.724ZM767.002 197.382C766.994 197.398 766.994 197.413 767.002 197.429C767.016 197.437 767.031 197.437 767.046 197.437H769.879C769.893 197.437 769.904 197.437 769.912 197.429C769.926 197.413 769.93 197.398 769.923 197.382L768.495 192.91C768.487 192.895 768.476 192.887 768.462 192.887C768.447 192.887 768.436 192.895 768.429 192.91L767.002 197.382ZM775.144 199.575C775.07 199.575 775.034 199.536 775.034 199.465V192.01C775.034 191.939 775.07 191.9 775.144 191.9H775.704C775.777 191.9 775.813 191.939 775.813 192.01V198.854C775.813 198.878 775.828 198.894 775.857 198.894H779.734C779.807 198.894 779.844 198.933 779.844 199.003V199.465C779.844 199.536 779.807 199.575 779.734 199.575H775.144Z" fill="white"/>
  <path d="M675.478 102.3C675.262 101.454 674.668 100.248 674.074 99.3119L672.4 99.9419C672.976 100.896 673.534 102.174 673.696 102.984L675.478 102.3ZM665.542 111.876V110.382H664.228V109.32H665.578V107.79H664.768C665.002 107.25 665.308 106.458 665.632 105.684L664.282 105.378C664.156 105.99 663.886 106.908 663.67 107.538L664.696 107.79H662.212L663.094 107.502C663.022 106.962 662.716 106.08 662.428 105.468L661.312 105.792C661.6 106.422 661.852 107.232 661.924 107.79H661.258V109.32H662.608V110.382H661.276V111.876H662.608V114.738H664.228V111.876H665.542ZM675.172 105.648V103.686H672.274V98.7539H670.186V103.686H667.99V105.648H670.15C670.024 108.042 669.556 110.868 667.756 113.1V103.542H664.408V102.048H668.008V100.248H664.408V98.7719H662.392V100.248H658.918V102.048H662.392V103.542H659.116V115.512H660.88V105.234H665.956V113.514C665.956 113.694 665.902 113.748 665.74 113.748C665.56 113.766 665.056 113.766 664.588 113.748C664.804 114.198 665.02 114.954 665.056 115.44C665.992 115.44 666.64 115.404 667.144 115.116C667.342 115.008 667.486 114.864 667.576 114.684C667.918 114.99 668.242 115.314 668.44 115.566C670.024 113.946 670.96 112.002 671.518 110.022C672.076 112.308 672.886 114.234 674.164 115.584C674.488 115.008 675.172 114.198 675.658 113.82C673.894 112.164 672.994 109.068 672.526 105.648H675.172Z" fill="black"/>
  <path d="M610.594 160.324C610.654 160.324 610.702 160.348 610.738 160.396C610.786 160.432 610.81 160.48 610.81 160.54V161.926C610.81 161.986 610.786 162.04 610.738 162.088C610.702 162.124 610.654 162.142 610.594 162.142H607.282C607.222 162.142 607.192 162.172 607.192 162.232V172.708C607.192 172.768 607.168 172.822 607.12 172.87C607.084 172.906 607.036 172.924 606.976 172.924H605.32C605.26 172.924 605.206 172.906 605.158 172.87C605.122 172.822 605.104 172.768 605.104 172.708V162.232C605.104 162.172 605.074 162.142 605.014 162.142H601.828C601.768 162.142 601.714 162.124 601.666 162.088C601.63 162.04 601.612 161.986 601.612 161.926V160.54C601.612 160.48 601.63 160.432 601.666 160.396C601.714 160.348 601.768 160.324 601.828 160.324H610.594Z" fill="white"/>
  <path d="M621.396 160.54C621.396 160.48 621.414 160.432 621.45 160.396C621.498 160.348 621.552 160.324 621.612 160.324H623.268C623.328 160.324 623.376 160.348 623.412 160.396C623.46 160.432 623.484 160.48 623.484 160.54V172.708C623.484 172.768 623.46 172.822 623.412 172.87C623.376 172.906 623.328 172.924 623.268 172.924H621.612C621.552 172.924 621.498 172.906 621.45 172.87C621.414 172.822 621.396 172.768 621.396 172.708V167.56C621.396 167.5 621.366 167.47 621.306 167.47H616.716C616.656 167.47 616.626 167.5 616.626 167.56V172.708C616.626 172.768 616.602 172.822 616.554 172.87C616.518 172.906 616.47 172.924 616.41 172.924H614.754C614.694 172.924 614.64 172.906 614.592 172.87C614.556 172.822 614.538 172.768 614.538 172.708V160.54C614.538 160.48 614.556 160.432 614.592 160.396C614.64 160.348 614.694 160.324 614.754 160.324H616.41C616.47 160.324 616.518 160.348 616.554 160.396C616.602 160.432 616.626 160.48 616.626 160.54V165.58C616.626 165.64 616.656 165.67 616.716 165.67H621.306C621.366 165.67 621.396 165.64 621.396 165.58V160.54Z" fill="white"/>
  <path d="M636.331 161.908C636.331 161.968 636.307 162.022 636.259 162.07C636.223 162.106 636.175 162.124 636.115 162.124H629.977C629.917 162.124 629.887 162.154 629.887 162.214V165.58C629.887 165.64 629.917 165.67 629.977 165.67H634.099C634.159 165.67 634.207 165.694 634.243 165.742C634.291 165.778 634.315 165.826 634.315 165.886V167.254C634.315 167.314 634.291 167.368 634.243 167.416C634.207 167.452 634.159 167.47 634.099 167.47H629.977C629.917 167.47 629.887 167.5 629.887 167.56V171.034C629.887 171.094 629.917 171.124 629.977 171.124H636.115C636.175 171.124 636.223 171.148 636.259 171.196C636.307 171.232 636.331 171.28 636.331 171.34V172.708C636.331 172.768 636.307 172.822 636.259 172.87C636.223 172.906 636.175 172.924 636.115 172.924H628.015C627.955 172.924 627.901 172.906 627.853 172.87C627.817 172.822 627.799 172.768 627.799 172.708V160.54C627.799 160.48 627.817 160.432 627.853 160.396C627.901 160.348 627.955 160.324 628.015 160.324H636.115C636.175 160.324 636.223 160.348 636.259 160.396C636.307 160.432 636.331 160.48 636.331 160.54V161.908Z" fill="white"/>
  <path d="M657.243 172.924C657.123 172.924 657.045 172.864 657.009 172.744L656.415 170.818C656.391 170.77 656.361 170.746 656.325 170.746H651.573C651.537 170.746 651.507 170.77 651.483 170.818L650.889 172.744C650.853 172.864 650.775 172.924 650.655 172.924H648.855C648.783 172.924 648.729 172.906 648.693 172.87C648.657 172.822 648.651 172.756 648.675 172.672L652.581 160.504C652.617 160.384 652.695 160.324 652.815 160.324H655.065C655.185 160.324 655.263 160.384 655.299 160.504L659.223 172.672C659.235 172.696 659.241 172.726 659.241 172.762C659.241 172.87 659.175 172.924 659.043 172.924H657.243ZM652.041 169C652.029 169.072 652.053 169.108 652.113 169.108H655.767C655.839 169.108 655.863 169.072 655.839 169L653.985 162.97C653.973 162.922 653.955 162.898 653.931 162.898C653.907 162.898 653.889 162.922 653.877 162.97L652.041 169Z" fill="white"/>
  <path d="M669.977 172.924C669.857 172.924 669.773 172.87 669.725 172.762L667.349 167.578C667.325 167.53 667.289 167.506 667.241 167.506H665.081C665.021 167.506 664.991 167.536 664.991 167.596V172.708C664.991 172.768 664.967 172.822 664.919 172.87C664.883 172.906 664.835 172.924 664.775 172.924H663.119C663.059 172.924 663.005 172.906 662.957 172.87C662.921 172.822 662.903 172.768 662.903 172.708V160.54C662.903 160.48 662.921 160.432 662.957 160.396C663.005 160.348 663.059 160.324 663.119 160.324H668.069C668.801 160.324 669.449 160.48 670.013 160.792C670.577 161.092 671.015 161.524 671.327 162.088C671.639 162.64 671.795 163.276 671.795 163.996C671.795 164.824 671.579 165.532 671.147 166.12C670.727 166.696 670.139 167.098 669.383 167.326C669.359 167.326 669.341 167.338 669.329 167.362C669.317 167.386 669.317 167.41 669.329 167.434L671.867 172.672C671.891 172.72 671.903 172.756 671.903 172.78C671.903 172.876 671.837 172.924 671.705 172.924H669.977ZM665.081 162.124C665.021 162.124 664.991 162.154 664.991 162.214V165.796C664.991 165.856 665.021 165.886 665.081 165.886H667.781C668.357 165.886 668.819 165.718 669.167 165.382C669.527 165.034 669.707 164.578 669.707 164.014C669.707 163.45 669.527 162.994 669.167 162.646C668.819 162.298 668.357 162.124 667.781 162.124H665.081Z" fill="white"/>
  <path d="M679.954 173.068C679.054 173.068 678.262 172.894 677.578 172.546C676.894 172.186 676.366 171.688 675.994 171.052C675.622 170.416 675.436 169.678 675.436 168.838V164.392C675.436 163.552 675.622 162.814 675.994 162.178C676.366 161.542 676.894 161.05 677.578 160.702C678.262 160.354 679.054 160.18 679.954 160.18C680.842 160.18 681.628 160.348 682.312 160.684C682.996 161.02 683.524 161.494 683.896 162.106C684.268 162.718 684.454 163.426 684.454 164.23C684.454 164.29 684.43 164.344 684.382 164.392C684.346 164.428 684.298 164.446 684.238 164.446L682.582 164.536C682.438 164.536 682.366 164.47 682.366 164.338C682.366 163.63 682.144 163.06 681.7 162.628C681.268 162.196 680.686 161.98 679.954 161.98C679.222 161.98 678.634 162.196 678.19 162.628C677.746 163.06 677.524 163.63 677.524 164.338V168.928C677.524 169.624 677.746 170.188 678.19 170.62C678.634 171.052 679.222 171.268 679.954 171.268C680.686 171.268 681.268 171.058 681.7 170.638C682.144 170.206 682.366 169.636 682.366 168.928C682.366 168.796 682.438 168.73 682.582 168.73L684.238 168.802C684.298 168.802 684.346 168.82 684.382 168.856C684.43 168.892 684.454 168.934 684.454 168.982C684.454 169.798 684.268 170.518 683.896 171.142C683.524 171.754 682.996 172.228 682.312 172.564C681.628 172.9 680.842 173.068 679.954 173.068Z" fill="white"/>
  <path d="M695.351 160.54C695.351 160.48 695.369 160.432 695.405 160.396C695.453 160.348 695.507 160.324 695.567 160.324H697.223C697.283 160.324 697.331 160.348 697.367 160.396C697.415 160.432 697.439 160.48 697.439 160.54V172.708C697.439 172.768 697.415 172.822 697.367 172.87C697.331 172.906 697.283 172.924 697.223 172.924H695.567C695.507 172.924 695.453 172.906 695.405 172.87C695.369 172.822 695.351 172.768 695.351 172.708V167.56C695.351 167.5 695.321 167.47 695.261 167.47H690.671C690.611 167.47 690.581 167.5 690.581 167.56V172.708C690.581 172.768 690.557 172.822 690.509 172.87C690.473 172.906 690.425 172.924 690.365 172.924H688.709C688.649 172.924 688.595 172.906 688.547 172.87C688.511 172.822 688.493 172.768 688.493 172.708V160.54C688.493 160.48 688.511 160.432 688.547 160.396C688.595 160.348 688.649 160.324 688.709 160.324H690.365C690.425 160.324 690.473 160.348 690.509 160.396C690.557 160.432 690.581 160.48 690.581 160.54V165.58C690.581 165.64 690.611 165.67 690.671 165.67H695.261C695.321 165.67 695.351 165.64 695.351 165.58V160.54Z" fill="white"/>
  <path d="M701.97 172.924C701.91 172.924 701.856 172.906 701.808 172.87C701.772 172.822 701.754 172.768 701.754 172.708V160.54C701.754 160.48 701.772 160.432 701.808 160.396C701.856 160.348 701.91 160.324 701.97 160.324H703.626C703.686 160.324 703.734 160.348 703.77 160.396C703.818 160.432 703.842 160.48 703.842 160.54V172.708C703.842 172.768 703.818 172.822 703.77 172.87C703.734 172.906 703.686 172.924 703.626 172.924H701.97Z" fill="white"/>
  <path d="M711.523 172.924C711.403 172.924 711.325 172.864 711.289 172.744L707.509 160.576L707.491 160.504C707.491 160.384 707.557 160.324 707.689 160.324H709.471C709.603 160.324 709.687 160.384 709.723 160.504L712.423 169.9C712.435 169.936 712.453 169.954 712.477 169.954C712.501 169.954 712.519 169.936 712.531 169.9L715.213 160.504C715.249 160.384 715.333 160.324 715.465 160.324H717.211C717.283 160.324 717.337 160.348 717.373 160.396C717.409 160.444 717.415 160.504 717.391 160.576L713.557 172.744C713.521 172.864 713.443 172.924 713.323 172.924H711.523Z" fill="white"/>
  <path d="M729.425 161.908C729.425 161.968 729.401 162.022 729.353 162.07C729.317 162.106 729.269 162.124 729.209 162.124H723.071C723.011 162.124 722.981 162.154 722.981 162.214V165.58C722.981 165.64 723.011 165.67 723.071 165.67H727.193C727.253 165.67 727.301 165.694 727.337 165.742C727.385 165.778 727.409 165.826 727.409 165.886V167.254C727.409 167.314 727.385 167.368 727.337 167.416C727.301 167.452 727.253 167.47 727.193 167.47H723.071C723.011 167.47 722.981 167.5 722.981 167.56V171.034C722.981 171.094 723.011 171.124 723.071 171.124H729.209C729.269 171.124 729.317 171.148 729.353 171.196C729.401 171.232 729.425 171.28 729.425 171.34V172.708C729.425 172.768 729.401 172.822 729.353 172.87C729.317 172.906 729.269 172.924 729.209 172.924H721.109C721.049 172.924 720.995 172.906 720.947 172.87C720.911 172.822 720.893 172.768 720.893 172.708V160.54C720.893 160.48 720.911 160.432 720.947 160.396C720.995 160.348 721.049 160.324 721.109 160.324H729.209C729.269 160.324 729.317 160.348 729.353 160.396C729.401 160.432 729.425 160.48 729.425 160.54V161.908Z" fill="white"/>
  <path d="M1256.56 13.9999C1256.46 13.9999 1256.4 13.9532 1256.37 13.8599L1253.43 4.39589L1253.42 4.33989C1253.42 4.24656 1253.47 4.19989 1253.57 4.19989H1254.96C1255.06 4.19989 1255.13 4.24656 1255.16 4.33989L1257.26 11.6479C1257.27 11.6759 1257.28 11.6899 1257.3 11.6899C1257.32 11.6899 1257.33 11.6759 1257.34 11.6479L1259.43 4.33989C1259.45 4.24656 1259.52 4.19989 1259.62 4.19989H1260.98C1261.04 4.19989 1261.08 4.21856 1261.11 4.25589C1261.13 4.29322 1261.14 4.33989 1261.12 4.39589L1258.14 13.8599C1258.11 13.9532 1258.05 13.9999 1257.96 13.9999H1256.56Z" fill="white"/>
  <path d="M1272.32 4.24189C1272.4 4.21389 1272.47 4.19989 1272.53 4.19989H1273.88C1273.93 4.19989 1273.96 4.21856 1273.99 4.25589C1274.03 4.28389 1274.05 4.32122 1274.05 4.36789V13.8319C1274.05 13.8786 1274.03 13.9206 1273.99 13.9579C1273.96 13.9859 1273.93 13.9999 1273.88 13.9999H1272.59C1272.54 13.9999 1272.5 13.9859 1272.46 13.9579C1272.44 13.9206 1272.42 13.8786 1272.42 13.8319V5.87989C1272.42 5.86122 1272.41 5.84722 1272.39 5.83789C1272.38 5.81922 1272.36 5.81456 1272.34 5.82389L1270.94 6.25789C1270.92 6.26722 1270.9 6.27189 1270.87 6.27189C1270.83 6.27189 1270.8 6.25789 1270.77 6.22989C1270.75 6.20189 1270.74 6.16456 1270.74 6.11789L1270.7 5.19389C1270.7 5.10056 1270.74 5.03522 1270.81 4.99789L1272.32 4.24189Z" fill="white"/>
  <path d="M1285.42 14.0279C1285.13 14.0279 1284.9 13.9346 1284.71 13.7479C1284.52 13.5612 1284.43 13.3232 1284.43 13.0339C1284.43 12.7446 1284.52 12.5066 1284.71 12.3199C1284.9 12.1332 1285.13 12.0399 1285.42 12.0399C1285.71 12.0399 1285.95 12.1332 1286.12 12.3199C1286.31 12.5066 1286.4 12.7446 1286.4 13.0339C1286.4 13.3232 1286.31 13.5612 1286.12 13.7479C1285.94 13.9346 1285.7 14.0279 1285.42 14.0279Z" fill="white"/>
  <path d="M1298.8 12.5159C1298.78 12.5346 1298.77 12.5532 1298.77 12.5719C1298.78 12.5906 1298.8 12.5999 1298.83 12.5999H1302.82C1302.86 12.5999 1302.9 12.6186 1302.93 12.6559C1302.97 12.6839 1302.98 12.7212 1302.98 12.7679V13.8319C1302.98 13.8786 1302.97 13.9206 1302.93 13.9579C1302.9 13.9859 1302.86 13.9999 1302.82 13.9999H1296.8C1296.75 13.9999 1296.71 13.9859 1296.67 13.9579C1296.64 13.9206 1296.63 13.8786 1296.63 13.8319V12.8239C1296.63 12.7399 1296.66 12.6699 1296.71 12.6139C1297.42 11.8579 1298.24 10.9292 1299.18 9.82789L1299.83 9.05789C1300.69 8.05922 1301.12 7.30322 1301.12 6.78989C1301.12 6.40722 1300.99 6.09456 1300.72 5.85189C1300.45 5.60922 1300.1 5.48789 1299.67 5.48789C1299.24 5.48789 1298.89 5.60922 1298.62 5.85189C1298.36 6.09456 1298.22 6.41656 1298.22 6.81789V7.18189C1298.22 7.22856 1298.21 7.27056 1298.17 7.30789C1298.14 7.33589 1298.1 7.34989 1298.06 7.34989H1296.75C1296.71 7.34989 1296.67 7.33589 1296.63 7.30789C1296.6 7.27056 1296.59 7.22856 1296.59 7.18189V6.57989C1296.61 6.07589 1296.75 5.63722 1297.02 5.26389C1297.29 4.89056 1297.66 4.60122 1298.11 4.39589C1298.58 4.19056 1299.1 4.08789 1299.67 4.08789C1300.29 4.08789 1300.84 4.20456 1301.3 4.43789C1301.77 4.67122 1302.13 4.99322 1302.38 5.40389C1302.63 5.80522 1302.76 6.25789 1302.76 6.76189C1302.76 7.52722 1302.35 8.40922 1301.54 9.40789C1301.09 9.96789 1300.36 10.8079 1299.33 11.9279L1298.8 12.5159Z" fill="white"/>
  <path d="M1314.04 14.0279C1313.75 14.0279 1313.51 13.9346 1313.33 13.7479C1313.14 13.5612 1313.05 13.3232 1313.05 13.0339C1313.05 12.7446 1313.14 12.5066 1313.33 12.3199C1313.51 12.1332 1313.75 12.0399 1314.04 12.0399C1314.33 12.0399 1314.56 12.1332 1314.74 12.3199C1314.93 12.5066 1315.02 12.7446 1315.02 13.0339C1315.02 13.3232 1314.93 13.5612 1314.74 13.7479C1314.55 13.9346 1314.32 14.0279 1314.04 14.0279Z" fill="white"/>
  <path d="M1327.42 12.5159C1327.4 12.5346 1327.39 12.5532 1327.39 12.5719C1327.4 12.5906 1327.42 12.5999 1327.45 12.5999H1331.44C1331.48 12.5999 1331.52 12.6186 1331.55 12.6559C1331.58 12.6839 1331.6 12.7212 1331.6 12.7679V13.8319C1331.6 13.8786 1331.58 13.9206 1331.55 13.9579C1331.52 13.9859 1331.48 13.9999 1331.44 13.9999H1325.42C1325.37 13.9999 1325.33 13.9859 1325.29 13.9579C1325.26 13.9206 1325.25 13.8786 1325.25 13.8319V12.8239C1325.25 12.7399 1325.28 12.6699 1325.33 12.6139C1326.04 11.8579 1326.86 10.9292 1327.8 9.82789L1328.45 9.05789C1329.31 8.05922 1329.74 7.30322 1329.74 6.78989C1329.74 6.40722 1329.61 6.09456 1329.34 5.85189C1329.06 5.60922 1328.71 5.48789 1328.29 5.48789C1327.86 5.48789 1327.51 5.60922 1327.24 5.85189C1326.97 6.09456 1326.84 6.41656 1326.84 6.81789V7.18189C1326.84 7.22856 1326.82 7.27056 1326.79 7.30789C1326.76 7.33589 1326.72 7.34989 1326.68 7.34989H1325.37C1325.33 7.34989 1325.28 7.33589 1325.25 7.30789C1325.22 7.27056 1325.21 7.22856 1325.21 7.18189V6.57989C1325.22 6.07589 1325.37 5.63722 1325.64 5.26389C1325.91 4.89056 1326.27 4.60122 1326.73 4.39589C1327.2 4.19056 1327.72 4.08789 1328.29 4.08789C1328.91 4.08789 1329.46 4.20456 1329.92 4.43789C1330.39 4.67122 1330.75 4.99322 1331 5.40389C1331.25 5.80522 1331.38 6.25789 1331.38 6.76189C1331.38 7.52722 1330.97 8.40922 1330.16 9.40789C1329.71 9.96789 1328.98 10.8079 1327.95 11.9279L1327.42 12.5159Z" fill="white"/>
  </svg>
  
      </div>
  
    </div>
  </section>
  