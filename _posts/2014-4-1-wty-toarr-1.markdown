---
bg: "railss.jpg"
layout: post
title:  "数组"
crawlertitle: "How to use jekyll"
summary: "php"
date:   2015-12-29 12:00:47 +0700
categories: posts
tags: 'php'
author: redVi
---

#### PHP数组处理基础

     function arraySort($arr, $keys, $type = 'asc') {
            $keysvalue = $new_array = array();
            foreach ($arr as $k => $v){
                $keysvalue[$k] = $v[$keys];
            }
            $type == 'asc' ? asort($keysvalue) : arsort($keysvalue);
            reset($keysvalue);
            foreach ($keysvalue as $k => $v) {
               $new_array[$k] = $arr[$k];
            }
            return $new_array;
        }

    $arr[] = array("name"=>"1","time"=>1) ;

    $arr[] = array("name"=>"2","time"=>2);

    arraySort($arr,"time","desc");