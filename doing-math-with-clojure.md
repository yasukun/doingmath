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

### Projectile Motion

```clojure
(def ^:const g 9.8)

(defn interval-xy [u theta]
  (let [theta* (Math/toRadians theta)
        t-flight (/ (* 2 u (Math/sin theta*)) g)]
    (map (fn [t]
           [(* u (Math/cos theta*) t)
            (- (* u (Math/sin theta*) t) (* 0.5 g t t))])
         (range 0 t-flight 0.001))))

(defn run-trajectory []
  (println "Enter the initial velocity (m/s): ")
  (let [u (read-string (read-line))]
    (println "Enter the angle of projection (degrees): ")
    (let [theta (read-string (read-line))
          trajectory (interval-xy u theta)]
      (view (xy-plot (map first trajectory)
                     (map second trajectory)
                     :title "Projectile motion of a ball"
                     :x-label "x-coordinate"
                     :y-label "y-coordinate")))))
```

### Comparing the Trajectory at Different Initial Velocities

```clojure
(defn diff-trajectory []
  (let [theta 45
        u-list [20 40 60]
        init-data (interval-xy (first u-list) theta)
        chart (xy-plot (map first init-data)
                     (map second init-data)
                     :title "Projectile motion of a ball"
                     :x-label "x-coordinate"
                     :y-label "y-coordinate"
                     :series-label (first u-list)
                     :legend true)]
    (doseq [u (rest u-list)]
        (let [data (interval-xy u theta)]
          (add-lines chart (map first data) (map second data) :series-label u)))
    (view chart)))
```

### Programming Challenges

#### #5: Exploring the Relationship Between the Fibonacci Sequence and the Golden Ratio

```clojure
(defn fibo []
  (rest (map first (iterate (fn [[a b]] [b (+ a b)]) [0N 1N]))))

(defn draw-fibo [n]
  (let [ratio (map (fn [[a b]] (double (/ b a))) (partition 2 1 (take (+ n 2) (fibo))))
        chart (xy-plot (range 0 (count ratio)) (doall ratio)
                   :title "Ratio between consecutive Fibonacci numbers"
                   :x-label "No."
                   :y-label "Ratio")]
    (set-y-range chart 1.0 2.2)
    (view chart)))

```

## Chapter 2: Describing Data With Statics

### Finding the Mean

```clojure
(let [donations [100 60 70 90 900 100 200 500 500 503 600 1000 1200]]
  (println (str "Mean donation over the last " (count donations) " days is " (mean donations))))
```

### Finding the Mean

```clojure
(let [donations [100 60 70 900 100 200 500 500 503 600 1000 1200]]
  (println (str "Median donation over the last " (count donations) " days is " (median donations))))
```

### Finding the Mode and Creating a Frequency Table

```clojure
(defn mode [coll]
  (let [freqs (frequencies coll)
        occurrences (group-by second freqs)
        modes (last (sort occurrences))
        modes (->> modes
                   second
                   (map first))]
    modes))

(let [scores [7 8 9 2 10 9 9 9 9 4 5 6 1 5 6 7 8 6 1 10]
      mode (mode scores)
      pr-mode (apply str (interpose "," mode))]
  (println (str "The mode of the list of numbers is: " pr-mode)))

(use 'clojure.pprint)
(let [scores [7 8 9 2 10 9 9 9 9 4 5 6 1 5 6 7 8 6 1 10]
      freq-table (map (fn [[k v]] {:Number k :Frequency v}) (frequencies scores))]
  (print-table (sort-by :Frequency > freq-table)))
```

### Finding the Variance and Standard Deviation

```clojure
(let [donations [100 60 70 900 100 200 500 500 503 600 1000 1200]
      variance (/ (sum (map #(Math/pow (- % d-mean) 2) donations)) (count donations))]
  (println (str "The variance of the list of numbers is " variance))
  (println (str "The standard deviation of the list of numbers is " (Math/sqrt variance))))

```

## Chapter4: Algebra and symbolic math with sympy

### Defining symbols and symbolic operations
