# Introduction

This project was made with JetBrain's [WriterSide](https://www.jetbrains.com/es-es/writerside/)

For seeing it, you can visit [danielalmazan.github.io](https://danielalmazan.github.io/home)

Even I am not planning to get rid of it, just in case it falls down, I have prepared this [Dockerfile](Dockerfile) to run it from there

You just have to build it

```bash
docker build -t help-website .
```

And run it

```bash
docker run -dit -p 8080:80 help-website
```

And you will be able to visit it at [localhost:8080](http://localhost:8080)

I also included the compiled [PDF](Writerside/pdfSourceTASKLYNX-DOCS.pdf) and [HTML](Writerside/pdfSourceTASKLYNX-DOCS.html) with the last version of the documentation
