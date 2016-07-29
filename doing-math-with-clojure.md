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

(defn print-menu []
        (println "1. Kilometers to Miles")
        (println "2. Miles to Kilometers")
        (println "3. Kilograms to Pounds")
        (println "4. Pounds to Kilograms")
        (println "5. Celsius to Fahrenheit")
        (println "6. Fahrenheit to Celsius"))

(defn disp-result [num]
  (let [msgs {:1 ["distance" "kilometers" :km "Distance" "miles"]
              :2 ["distance" "miles" :miles "Distance" "kilometers"]
              :3 ["weight" "kilograms" :kg "Weight" "pounds"]
              :4 ["weight" "pounds" :kg "Weight" "kilograms"]
              :5 ["temperature" "celsius" :celsius "Temperature" "fahrenheit"]
              :6 ["temperature" "fahrenheit" :celsius "Temperature" "celsius"]}
        [m1 m2 k m3 m4] (get msgs (keyword num))]
    (println (format "Enter %s in %s: " m1 m2))
    (let [val (read-string (read-line))]
      (println (format "%s in %s: %s" m3 m4 (str (:val (cv-unit {:Unit k :val val}))))))))

(defn run-unit-conv []
  (print-menu)
  (println "Which conversion would you like to do?")
  (let [input (read-line)]
    (disp-result input)))

```

## Chapter 2: Visualizing Data with Graphs

### Creating Graphs with ~~Matplotlib~~ incanter

load incanter

```clojure
(use '(incanter core stats charts io datasets))
(def x-numbers [1 2 3])
(def y-numbers [2 4 6])
(view (xy-plot x-numbers y-numbers))
```
marker

```clojure
(view (xy-plot x-numbers y-numbers :points true))
```

### Graphing the Average Annual Temperature in New York City

```clojure
(def nyc-temp [53.9 56.3 56.4 53.4 54.5 55.8 56.8 55.0 55.3 54.0 56.7 56.4 57.3])
(view (xy-plot (range (count nyc-temp)) nyc-temp  :points true))
(view (xy-plot (range 2000 2013) nyc-temp  :points true))
```
