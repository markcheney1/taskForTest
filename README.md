# taskForTest
.....


package main

import (
	"os"
	"bufio"
	"strings"
	"strconv"
	"fmt"
)
var(
	CLOSE_WAIT_MAX =0
	CLOSE_WAIT_AVG=0
	count_CLOSE_WAIT=0

	ESTABLISHED_MAX=0
	ESTABLISHED_AVG=0
	count_ESTABLISHED=0

	TIME_WAIT_MAX=0
	TIME_WAIT_AVG=0
	count_TIME_WAIT=0

)

func count(f *os.File){
	input:=bufio.NewScanner(f)
	for input.Scan(){
		content:=input.Text()
		if strings.Contains(content,"time_wait count:"){
			continue
		}
		if strings.Contains(content,"CLOSE_WAIT"){
			num,_:=strconv.Atoi(strings.Split(content," ")[1])
			if num>CLOSE_WAIT_MAX{
				CLOSE_WAIT_MAX=num
			}
			CLOSE_WAIT_AVG+=num
			count_CLOSE_WAIT++
			continue
		}
		if strings.Contains(content,"ESTABLISHED"){
			num,_:=strconv.Atoi(strings.Split(content," ")[1])
			if num>ESTABLISHED_MAX{
				ESTABLISHED_MAX=num
			}
			ESTABLISHED_AVG+=num
			count_ESTABLISHED++
			continue
		}
		if strings.Contains(content,"TIME_WAIT"){
			num,_:=strconv.Atoi(strings.Split(content," ")[1])
			if num>TIME_WAIT_MAX{
				TIME_WAIT_MAX=num
			}
			TIME_WAIT_AVG+=num
			count_TIME_WAIT++
			continue
		}
	}
	fmt.Println("file name: ",f.Name())
	fmt.Println("CLOSE_WAIT_MAX: ",CLOSE_WAIT_MAX)
	fmt.Println("CLOSE_WAIT_AVG: ",CLOSE_WAIT_AVG/count_CLOSE_WAIT)
	fmt.Println()
	fmt.Println("ESTABLISHED_MAX: ",ESTABLISHED_MAX)
	fmt.Println("ESTABLISHED_AVG: ",ESTABLISHED_AVG/count_ESTABLISHED)
	fmt.Println()
	fmt.Println("TIME_WAIT_MAX: ",TIME_WAIT_MAX)
	fmt.Println("TIME_WAIT_AVG: ",TIME_WAIT_AVG/count_TIME_WAIT)

}

func main() {
/*
	filename:=os.Args[1:]
	if len(filename)==0{
		fmt.Println("please input filepath")
		//return
	}
	filename=append(filename,"C:\\test\\beego.txt")
	counts:=make(map[string]int)
	for _,v:=range filename{
		data,err:=ioutil.ReadFile(v)
		if err!=nil{
			fmt.Println("read file failed")
			continue
		}
		for _,v := range strings.Split(string(data),"\n"){
			counts[v]++
		}
	}
	fmt.Println(len(counts))
	for k,mk:=range counts{
		fmt.Println(k," ",mk)
	}
*/
/*	s:=make(map[string]int)
	s["dd"]++
	s["dd"]++
	s["dd"]++
	s["ss"]++
	s["ss"]++
	s["ss"]++
	s["ss"]++
	for k,v:=range s{
		fmt.Println(k," ",v)
	}*/
/*	s:=[]string{"w","d"}
	str:=strings.Join(s,":")
	fmt.Printf("%c,%c,%c\n",str[0],str[1],str[2])
	fmt.Println(time.Now().UTC().Format("2006-01-02"))
	time.ANSIC*/
	file:=os.Args[1:]
	if len(file)==0{
		fmt.Println("file name is null,please input filename")
		return
	}
	for _,path:=range file{
		f,_:=os.Open(path)
		count(f)
		f.Close()
	}

}
