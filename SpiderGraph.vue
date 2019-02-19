<template>
  <div id="questionsPage">
    <canvas ref="graph"></canvas>
  </div>
</template>

<script>
const numberOfQuestion = 10;

export default {
  name: "SpiderGraph",
  props: ["qid", "predefinedRatings", "dataRatings", "predefinedRatingsColor", "predefinedRatingsGridColor", "dataRatingsColor", "dataRatingsColorGridColor", "width", "height", "numberOfDataPoints"],
  mounted() {
    this.$refs["graph"].height = this.height!=undefined? this.height:500;
    this.$refs["graph"].width = this.width!=undefined? this.width:500;
    this.context = this.$refs["graph"].getContext("2d");
    if(this.numberOfDataPoints==undefined) {
      this.numberOfDataPoints = 10;
    }
    this.draw();
  },
  render() {
    this.draw();
  },
  methods: {
    draw() {
      if (!this.context) return;
      const ctx = this.context;
      ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
      ctx.beginPath();
      let angel = -1.5715+(6.286/this.numberOfDataPoints/2);
      for (let i = 0; i < this.numberOfDataPoints; i++) {
        this.lineAtAngle(ctx.canvas.width/2, ctx.canvas.height/2, 260/600*ctx.canvas.height, angel);
        this.drawNumber(i+1, angel);
        angel += 6.286/this.numberOfDataPoints;
      }
      this.drawMiddleCircle();
      let radious = 50/600*ctx.canvas.height+20/600*ctx.canvas.height;
      for (let i = 0; i < 10; i++) {
        this.drawOuter(radious);
        radious += 20/600*ctx.canvas.height;
      }
      console.log(this.qid);
      this.getPredefinedRatings();
      this.getRatings();
    },
    lineAtAngle(x1, y1, length, angle) {
      const ctx = this.context;
      ctx.beginPath();
      ctx.moveTo(x1, y1);
      ctx.strokeStyle = "#BBBBBB";
      ctx.lineTo(x1 + length * Math.cos(angle), y1 + length * Math.sin(angle));
      ctx.stroke();
    },
    drawMiddleCircle() {
      const ctx = this.context;
      ctx.beginPath();
      ctx.arc(ctx.canvas.width/2, ctx.canvas.height/2, 50/600*ctx.canvas.height, 0, 2 * Math.PI, false);
      ctx.fillStyle = "white";
      ctx.fill();
      ctx.lineWidth = 1;
      ctx.strokeStyle = "#BBBBBB";
      ctx.stroke();
    },
    drawOuter(radious) {
      const ctx = this.context;
      ctx.beginPath();
      ctx.arc(ctx.canvas.width/2, ctx.canvas.height/2, radious, 0, 2 * Math.PI, false);
      ctx.lineWidth = 1;
      ctx.strokeStyle = "#BBBBBB";
      ctx.stroke();
    },
    drawNumber(i, angle) {
      const ctx = this.context;
      ctx.beginPath();
      ctx.font = (20/600*ctx.canvas.width)+"px Futura";
      ctx.fillStyle = "black";
      let x = ctx.canvas.width/2 + 270/600*ctx.canvas.width * Math.cos(angle);
      let y = ctx.canvas.height/2 + 270/600*ctx.canvas.height * Math.sin(angle);
      if (i == 6) {
        y = y + 13/600*ctx.canvas.width;
        x = x - 7/600*ctx.canvas.width;
      }
      if (i == 7 || i == 8) x = x - 10/600*ctx.canvas.width;
      if (i == 3 || i == 8) y = y + 6/600*ctx.canvas.width;
      if (i == 3) x = x - 3/600*ctx.canvas.width;
      if (i == 8) x = x + 1/600*ctx.canvas.width;
      if (i == 10) x = x - 10/600*ctx.canvas.width;
      if (i == 9) {
        x = x - 7/600*ctx.canvas.width;
        y = y + 4/600*ctx.canvas.width;
      }
      if (i == 7) {
        x = x - 3/600*ctx.canvas.width;
        y = y + 6/600*ctx.canvas.width;
      }
      if (i == 6) {
        y = y + 1/600*ctx.canvas.width;
        x = x - 2/600*ctx.canvas.width;
      }
      if (i == 5) {
        y = y + 12/600*ctx.canvas.width;
        x = x - 3/600*ctx.canvas.width;
      }
      if (i == 4) {
        y = y + 7/600*ctx.canvas.width;
        x = x - 3/600*ctx.canvas.width;
      }
      if (i == 11) {
        y = y - 1/600*ctx.canvas.width;
        x = x - 5/600*ctx.canvas.width;
      }
      if (i == 12) {
        y = y + 2/600*ctx.canvas.width;
        x = x - 1/600*ctx.canvas.width;
      }
      ctx.fillText(i > 10 ? i - 10 : i, x, y);
    },
    drawRating(i, angle, color) {
      const ctx = this.context;
      let x = ctx.canvas.width/2 + (50/600*ctx.canvas.width + 20/600*ctx.canvas.width * i) * Math.cos(angle);
      let y = ctx.canvas.height/2 + (50/600*ctx.canvas.height + 20/600*ctx.canvas.height * i) * Math.sin(angle);
      ctx.beginPath();
      ctx.rect(x - 10/600*ctx.canvas.width, y - 10/600*ctx.canvas.height, 20/600*ctx.canvas.width, 20/600*ctx.canvas.height);
      ctx.fillStyle = color;
      ctx.fill();
    },
    drawRatingConnector(i, angle, i1, angle1, color) {
      const ctx = this.context;
      let x = ctx.canvas.width/2 + (50/600*ctx.canvas.width + 20/600*ctx.canvas.width * i) * Math.cos(angle);
      let y = ctx.canvas.height/2 + (50/600*ctx.canvas.height + 20/600*ctx.canvas.height * i) * Math.sin(angle);
      let x1 = ctx.canvas.width/2 + (50/600*ctx.canvas.width + 20/600*ctx.canvas.width * i1) * Math.cos(angle1);
      let y1 = ctx.canvas.height/2 + (50/600*ctx.canvas.height + 20/600*ctx.canvas.height * i1) * Math.sin(angle1);
      ctx.beginPath();
      ctx.moveTo(x, y);
      ctx.lineTo(x1, y1);
      ctx.lineWidth = 2;
      ctx.strokeStyle = color;
      ctx.stroke();
    },
    drawRatingConnectorPlolygon(i, angle, i1, angle1, color) {
      const ctx = this.context;
      let x = ctx.canvas.width/2 + (50/600*ctx.canvas.width + 20/600*ctx.canvas.width * i) * Math.cos(angle);
      let y = ctx.canvas.height/2 + (50/600*ctx.canvas.height + 20/600*ctx.canvas.height * i) * Math.sin(angle);
      let x1 = ctx.canvas.width/2 + (50/600*ctx.canvas.width + 20/600*ctx.canvas.width * i1) * Math.cos(angle1);
      let y1 = ctx.canvas.height/2 + (50/600*ctx.canvas.height + 20/600*ctx.canvas.height * i1) * Math.sin(angle1);
      ctx.beginPath();
      ctx.moveTo(x, y);
      ctx.lineTo(x1, y1);
      ctx.lineTo(ctx.canvas.width/2, ctx.canvas.height/2);
      ctx.lineWidth = 2;
      ctx.closePath();
      ctx.fillStyle = color;
      ctx.fill();
    },
    getRatings() {
      this.ratings = [];
      if(this.dataRatings!=undefined && this.dataRatings.length>0) {
        this.ratings = this.dataRatings;
      }
      let angel = -1.5715+(6.286/this.numberOfDataPoints/2);
      for (let i = 0; i < this.ratings.length && i<this.numberOfDataPoints; i++) {
        this.drawRating(this.ratings[i], angel, this.dataRatingsColor!=undefined ? this.dataRatingsColor: "#c40d1e");
        if (i > 0) {
          this.drawRatingConnector(
            this.ratings[i-1],
            angel - 6.286/this.numberOfDataPoints,
            this.ratings[i],
            angel,
            this.dataRatingsColor!=undefined ? this.dataRatingsColor: "#c40d1e"
          );
          this.drawRatingConnectorPlolygon(
            this.ratings[i-1],
            angel - 6.286/this.numberOfDataPoints,
            this.ratings[i],
            angel,
            this.dataRatingsColorGridColor!=undefined ? this.dataRatingsColorGridColor: "rgba(196,11,30, 0.2)"
          );
        }
        if (i == this.numberOfDataPoints-1) {
          this.drawRatingConnector(
            this.ratings[i],
            angel,
            this.ratings[0],
            -1.5715+(6.286/this.numberOfDataPoints/2),
            this.dataRatingsColor!=undefined ? this.dataRatingsColor: "#c40d1e"
          );
          this.drawRatingConnectorPlolygon(
            this.ratings[i],
            angel,
            this.ratings[0],
            -1.5715+(6.286/this.numberOfDataPoints/2),
            this.dataRatingsColorGridColor!=undefined ? this.dataRatingsColorGridColor: "rgba(196,11,30, 0.2)"
          );
        }
        angel += 6.286/this.numberOfDataPoints;
      }
    },
    getPredefinedRatings() {
      if(this.predefinedRatings!=undefined && this.predefinedRatings.length>0) {
        let angel = -1.5715+(6.286/this.numberOfDataPoints/2);
        for (let i = 0; i < this.predefinedRatings.length && i<this.numberOfDataPoints; i++) {
          this.drawRating(this.predefinedRatings[i], angel, this.predefinedRatingsColor!=undefined ? this.predefinedRatingsColor: "#F7A61B");
          if (i > 0) {
            this.drawRatingConnector(
              this.predefinedRatings[i-1],
              angel - 6.286/this.numberOfDataPoints,
              this.predefinedRatings[i],
              angel,
              this.predefinedRatingsColor!=undefined ? this.predefinedRatingsColor: "#F7A61B"
            );
            this.drawRatingConnectorPlolygon(
              this.predefinedRatings[i-1],
              angel - 6.286/this.numberOfDataPoints,
              this.predefinedRatings[i],
              angel,
              this.predefinedRatingsGridColor!=undefined ? this.predefinedRatingsGridColor: "rgba(247,166,27, 0.2)"
            );
          }
          if (i == this.numberOfDataPoints-1) {
            this.drawRatingConnector(
              this.predefinedRatings[i],
              angel,
              this.predefinedRatings[0],
              -1.5715+(6.286/this.numberOfDataPoints/2),
              this.predefinedRatingsColor!=undefined ? this.predefinedRatingsColor: "#F7A61B"
            );
            this.drawRatingConnectorPlolygon(
              this.predefinedRatings[i],
              angel,
              this.predefinedRatings[0],
              -1.5715+(6.286/this.numberOfDataPoints/2),
              this.predefinedRatingsGridColor!=undefined ? this.predefinedRatingsGridColor: "rgba(247,166,27, 0.2)"
            );
          }
          angel += 6.286/this.numberOfDataPoints;
        }
      }
    }
  }
};
</script>
