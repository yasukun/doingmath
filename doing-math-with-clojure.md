# Doing math with clojure

## Chapter 1: Working with Numbers



### Programming Challenges

#### 3: Enhanced Unit Converter

```Clojure
(defmulti cv-unit :Unit)
(defmethod cv-unit :km [d] {:Unit :miles :val (/ (:val d) 1.609)})
(defmethod cv-unit :miles [d] {:Unit :km :val (* (:val d) 1.609)})
(defmethod cv-unit :celsius [d] {:Unit :fahrenheit :val (+ (* (:val d) (/ 9.0 5)) 32)})
(defmethod cv-unit :fahrenheit [d] {:Unit :celsius :val (* (- (:val d) 32) (/ 5.0 9))})
(defmethod cv-unit :kg [d] {:Unit :pound :val (/ (:val d) 0.4536)})
(defmethod cv-unit :pound [d] {:Unit :kg :val (* (:val d) 0.4536)})
```
