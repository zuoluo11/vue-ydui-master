<template>
    <yd-layout title="Slider">
        <yd-navbar slot="navbar" title="首页">
            <router-link to="/" slot="left">
                <yd-navbar-back-icon></yd-navbar-back-icon>
            </router-link>
        </yd-navbar>
        <div class="demo-small-pitch">
            <yd-slider autoplay="3000">
                <yd-slider-item>
                    <a href="http://www.ydcss.com">
                        <img src="http://static.ydcss.com/uploads/ydui/1.jpg">
                    </a>
                </yd-slider-item>
                <yd-slider-item>
                    <a href="http://www.ydcss.com">
                        <img src="http://static.ydcss.com/uploads/ydui/2.jpg">
                    </a>
                </yd-slider-item>
                <yd-slider-item>
                    <a href="http://www.ydcss.com">
                        <img src="http://static.ydcss.com/uploads/ydui/3.jpg">
                    </a>
                </yd-slider-item>
            </yd-slider>

            <yd-tabbar slot="tabbar" :fixed="true">
                <yd-tabbar-item title="首页" link="/" active>
                    <i slot="icon" class="demo-icons-weixin"></i>
                    <yd-badge slot="badge" type="danger">2</yd-badge>
                </yd-tabbar-item>
                <yd-tabbar-item title="今日" link="/">
                    <i slot="icon" class="demo-icons-contact"></i>
                </yd-tabbar-item>
                <yd-tabbar-item title="全部" link="/allalarm" dot>
                    <i slot="icon" class="demo-icons-discover"></i>
                </yd-tabbar-item>
                <yd-tabbar-item title="新增" link="/add">
                    <i slot="icon" class="demo-icons-me"></i>
                </yd-tabbar-item>
                
            </yd-tabbar>
        </div>
    </yd-layout>
</template>
