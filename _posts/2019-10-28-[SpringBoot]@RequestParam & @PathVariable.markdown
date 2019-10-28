---
layout: post
title: "[SpringBoot]@RequestParam & @PathVariable"
subtitle: "파라미터 값 넘겨받기"
categories: study
tags: springboot
---

## 파라미터 값을 넘겨받는 Annotation
###1. @RequestParam
 - 파라미터의 key , value 값을 함께 전달하는 방식
 예) http://localhost:8080/board?boardIdx=8
```
@RequestMapping("/board")
public ModelAndView openBoardDetail(@RequestParam("boardIdx") int boardIdx) throws Exception{
	ModelAndView mv = new ModelAndView("/board/restBoardDetail");	
	BoardDto board = boardService.selectBoardDetail(boardIdx);
	mv.addObject("board", board);
	log.debug(boardIdx); //  ----> 8이 출력됨
	return mv;
}
```

###2. @PathVariable
 - REST api에서 값을 호출할 때 많이 사용되는 방식
 예) http://localhost:8080/board/8
```
@RequestMapping(value="/board/{boardIdx}", method=RequestMethod.GET)
public ModelAndView openBoardDetail(@PathVariable("boardIdx") int boardIdx) throws Exception{
	ModelAndView mv = new ModelAndView("/board/restBoardDetail");	
	BoardDto board = boardService.selectBoardDetail(boardIdx);
	mv.addObject("board", board);
	log.debug(boardIdx); //  ----> 8이 출력됨
	return mv;
}
 ```
 
* @RequestParam , @PathVariable 동시 사용해 파라미터 값을 받는 것 가능
