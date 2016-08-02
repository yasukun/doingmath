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

```clojure
(use '(incanter core stats charts io datasets))
(def x-numbers [1 2 3])
(def y-numbers [2 4 6])
(view (xy-plot x-numbers y-numbers))
```
#### marker

```clojure
(view (xy-plot x-numbers y-numbers :points true))
```

### Graphing the Average Annual Temperature in New York City

```clojure
(def nyc-temp [53.9 56.3 56.4 53.4 54.5 55.8 56.8 55.0 55.3 54.0 56.7 56.4 57.3])
(view (xy-plot (range (count nyc-temp)) nyc-temp  :points true))
(view (xy-plot (range 2000 2013) nyc-temp  :points true))
```

### Comparing the Monthly Temperature Trends of New York City

```clojure
(def nyc-temp-2000 [31.3 37.3 47.2 51.0 63.5 71.3 72.3 72.7 66.0 57.0 45.3 31.1])
(def nyc-temp-2006 [40.9 35.7 43.1 55.7 63.1 71.0 77.9 75.8 66.6 56.2 51.9 43.6])
(def nyc-temp-2012 [37.3 40.9 50.9 54.8 65.1 71.0 78.8 76.7 68.8 58.0 43.9 41.5])
(def months (range 1 13))
(def chart1 (xy-plot months nyc-temp-2000
                     :title "Average monthly temperature in NYC"
                     :x-label "Month"
                     :y-label "Temperature"
                     :points true :legend true :series-label "2000"))
(view chart1)
(add-lines chart1 months nyc-temp-2006 :points true :series-label "2006")
(add-lines chart1 months nyc-temp-2012 :points true :series-label "2012")
```

### Customizing the Axes

```clojure
(def nyc-temp [53.9 56.3 56.4 53.4 54.5 55.8 56.8 55.0 55.3 54.0 56.7 56.4 57.3])
(def chart (xy-plot (range 2000 2013) nyc-temp  :points true))
(view chart)
(set-y-range chart 0 58)
```

### Newton's Law of Universal Gravitation

```clojure
(def r (range 100 1001 50))
(def generate-f-r
  (let [G (* 6.674 (Math/pow 10 -11))
        m1 0.5
        m2 1.5]
    (map #(/ (* G m1 m2) (Math/pow % 2)) r)))
(def chart (xy-plot r generate-f-r :points true
                    :title "Gravitational force and distance"
                    :x-label "Distance in meters"
                    :y-label "Gravitational force in newtons'"))
(set-y-range chart 0 6.0E-15)
(view chart)
```
