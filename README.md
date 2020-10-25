# Refactoring
My PA4 [Covid19-Tracker](https://github.com/OOP2020/pa4-bhatara007) project from Programing 2
## In setAll() method in WorldController.java

**Before refactor:**
```java
public void setAll(){

        //set percentage fill color.
        t1.setStyle("-fx-fill: forestgreen");
        t2.setStyle("-fx-fill: forestgreen");
        t3.setStyle("-fx-fill: forestgreen");
        t4.setStyle("-fx-fill: forestgreen");

        // formatted String of the values for display.
        newCase = String.format("%,d", Integer.parseInt(worldDataToday[2]));
        newDeaths = String.format("%,d", Integer.parseInt(worldDataToday[3]));
        totalCases = String.format("%,d", Integer.parseInt(worldDataToday[4]));
        totalDeaths = String.format("%,d", Integer.parseInt(worldDataToday[5]));

        // set a value of divide attributes.
        divide1 = convertDouble(getWorldDataYesterday[indexOfTotalCaseIndex]);
        divide2 = convertDouble(getWorldDataYesterday[indexOfNewCase]);
        divide3 = convertDouble(getWorldDataYesterday[indexOfNewDeath]);
        divide4 = convertDouble(getWorldDataYesterday[indexOfTotalDeath]);

        //fix a value of divide attributes when it equals zero.
        if (divide1 == 0) divide1 = 1;
        if (divide2 == 0) divide2 = 1;
        if (divide3 == 0) divide3 = 1;
        if (divide4 == 0) divide4 = 1;

        //calculate a percentage different.
        percent1 = (convertDouble(worldDataToday[indexOfTotalCaseIndex]) - convertDouble(getWorldDataYesterday[indexOfTotalCaseIndex])) * 100 / divide1;
        percent2 = (convertDouble(worldDataToday[indexOfNewCase]) - convertDouble(getWorldDataYesterday[indexOfNewCase])) * 100 / divide2;
        percent3 = (convertDouble(worldDataToday[indexOfNewDeath]) - convertDouble(getWorldDataYesterday[indexOfNewDeath])) * 100 / divide3;
        percent4 = (convertDouble(worldDataToday[indexOfTotalDeath]) - convertDouble(getWorldDataYesterday[indexOfTotalDeath])) * 100 / divide4;

        // formatted String of the percentage different value for display.
        diff1 = String.format("( %.2f", percent1);
        diff2 = String.format("( %.2f", percent2);
        diff3 = String.format("( %.2f", percent3);
        diff4 = String.format("( %.2f", percent4);

        if (percent1 > 0) {
            diff1 = String.format("( +%.2f", percent1);
            t1.setStyle("-fx-fill: red");
        }
        if (percent2 > 0) {
            diff2 = String.format("( +%.2f", percent2);
            t2.setStyle("-fx-fill: red");
        }
        if (percent3 > 0) {
            diff3 = String.format("( +%.2f", percent3);
            t3.setStyle("-fx-fill: red");
        }
        if (percent4 > 0) {
            diff4 = String.format("( +%.2f", percent4);
            t4.setStyle("-fx-fill: red");
        }

        //set text for display.
        lb1.setText(totalCases);
        lb2.setText(newCase);
        lb3.setText(totalDeaths);
        lb4.setText(newDeaths);
        text.setText("World");
        date.setText("Date: " + worldDataToday[0]);
        t1.setText(diff1 + "% )");
        t2.setText(diff2 + "% )");
        t3.setText(diff3 + "% )");
        t4.setText(diff4 + "% )");
    }
```
**Refactor**: This method has too many functions we need to exact method to separate
their work and make it easy to read and shorter code.

**After refactor:**
```java
   public void setAll(){

        //set percentage fill color.
        setPercentageDefaultColor();

        //get data
        getDataForDisplay();

        // set a value of divide attributes.
        setDivideValue();

        //calculate a percentage different.
        setPercentage();

        changePercentageColor();

        //set text for display.
        setText();
    }

    /**
     * set a default percentage different color is green
     */
    public void setPercentageDefaultColor(){

        t1.setStyle("-fx-fill: forestgreen");
        t2.setStyle("-fx-fill: forestgreen");
        t3.setStyle("-fx-fill: forestgreen");
        t4.setStyle("-fx-fill: forestgreen");
    }

    /**
     * if percentage different is more than 0
     * the color of text will change to red.
     */
    public void changePercentageColor(){
        if (percent1 > 0) {
            diff1 = String.format("( +%.2f", percent1);
            t1.setStyle("-fx-fill: red");
        }
        if (percent2 > 0) {
            diff2 = String.format("( +%.2f", percent2);
            t2.setStyle("-fx-fill: red");
        }
        if (percent3 > 0) {
            diff3 = String.format("( +%.2f", percent3);
            t3.setStyle("-fx-fill: red");
        }
        if (percent4 > 0) {
            diff4 = String.format("( +%.2f", percent4);
            t4.setStyle("-fx-fill: red");
        }
    }

    /**
     * get a data from API for display in summary window
     */
    public void getDataForDisplay(){
        newCase = String.format("%,d", Integer.parseInt(worldDataToday[2]));
        newDeaths = String.format("%,d", Integer.parseInt(worldDataToday[3]));
        totalCases = String.format("%,d", Integer.parseInt(worldDataToday[4]));
        totalDeaths = String.format("%,d", Integer.parseInt(worldDataToday[5]));
    }

    /**
     * set a divide value for calculate percentage different
     */
    public void setDivideValue(){
        // set a value of divide attributes.
        divide1 = convertDouble(getWorldDataYesterday[indexOfTotalCaseIndex]);
        divide2 = convertDouble(getWorldDataYesterday[indexOfNewCase]);
        divide3 = convertDouble(getWorldDataYesterday[indexOfNewDeath]);
        divide4 = convertDouble(getWorldDataYesterday[indexOfTotalDeath]);

        //fix a value of divide attributes when it equals zero.
        if (divide1 == 0) divide1 = 1;
        if (divide2 == 0) divide2 = 1;
        if (divide3 == 0) divide3 = 1;
        if (divide4 == 0) divide4 = 1;
    }

    /**
     * calculate percentage different for display.
     */
    public void setPercentage(){
        percent1 = (convertDouble(worldDataToday[indexOfTotalCaseIndex]) - convertDouble(getWorldDataYesterday[indexOfTotalCaseIndex])) * 100 / divide1;
        percent2 = (convertDouble(worldDataToday[indexOfNewCase]) - convertDouble(getWorldDataYesterday[indexOfNewCase])) * 100 / divide2;
        percent3 = (convertDouble(worldDataToday[indexOfNewDeath]) - convertDouble(getWorldDataYesterday[indexOfNewDeath])) * 100 / divide3;
        percent4 = (convertDouble(worldDataToday[indexOfTotalDeath]) - convertDouble(getWorldDataYesterday[indexOfTotalDeath])) * 100 / divide4;

        // formatted String of the percentage different value for display.
        diff1 = String.format("( %.2f", percent1);
        diff2 = String.format("( %.2f", percent2);
        diff3 = String.format("( %.2f", percent3);
        diff4 = String.format("( %.2f", percent4);

    }

    /**
     * set text for display.
     */
    public void setText(){
        lb1.setText(totalCases);
        lb2.setText(newCase);
        lb3.setText(totalDeaths);
        lb4.setText(newDeaths);
        text.setText("World");
        date.setText("Date: " + worldDataToday[0]);
        t1.setText(diff1 + "% )");
        t2.setText(diff2 + "% )");
        t3.setText(diff3 + "% )");
        t4.setText(diff4 + "% )");
    }
```

## For-loop statement at setAll() method in LineChartController.java
**Before refactor:**
```java
    for (int i = 1; i < datee.size(); i++) {
        XYChart.Data<String, Number> data = new XYChart.Data<String, Number>(String.valueOf(datee.get(i)), Integer.parseInt(confirmCase.get(i)));
        series.getData().add(data);
    }
```
**Refactor**: This problem in line 2 this statement too long and hard to understand we need to 
create a temp variable make the code to be longer but easy to read and understand. 

**After refactor:**
```java
    for (int i = 1; i < datee.size(); i++) {
        String date = String.valueOf(datee.get(i));
        int confirmCasee = Integer.parseInt(confirmCase.get(i));

        XYChart.Data<String, Number> data = new XYChart.Data<String, Number>(date, confirmCasee);

        series.getData().add(data);
    }
```

## In LineChartController.java and BarChartController.java
**Problem:** LineChartController.java and BarChartController,java are almost the same behavior
we want to create some method only one time and it can use for 2 class

### First we need to make a BarchartController class is a sub-class from LineChartController class

```java
public class BarChartController extends LineChartController implements Initializable {
        //a lot of java code....
}
```
**Refactor**: we need to extract method from setAll() method in LineChartController.java and if the BarChartController class is
sub-class of LineChartController class it can use the method that we already extracted as well that make code be shorter and easy to understand. 

**Before refactor (In LineChartController.java):**
```java
    public void setAll() {
        try {
            alert.setText("");
            lineChart.setTitle(countryName);

            confirmCase = gd.getCountryConfirmCase(graphType, countryName);
            showType.setValue(graphType);

            cb2.setValue(datee.get(datee.size() - 1));
            casee = confirmCase.get(confirmCase.size() - 1);

            for (int i = 1; i < datee.size(); i++) {
                XYChart.Data<String, Number> data = new XYChart.Data<String, Number>(String.valueOf(datee.get(i)), Integer.parseInt(confirmCase.get(i)));
                series.getData().add(data);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        lb1.setText(String.format("%s : %,d cases", graphType, Integer.parseInt(casee)));

    }
```

**After refactor (In LineChartController.java):**
```java
    public void chartSet(Chart chartType) throws Exception {
        alert.setText("");
        lineChart.setTitle(countryName);

        confirmCase = gd.getCountryConfirmCase(graphType, countryName);
        showType.setValue(graphType);

        cb2.setValue(datee.get(datee.size() - 1));
        casee = confirmCase.get(confirmCase.size() - 1);
    }

    public void addDataToChart(XYChart.Series series, ArrayList datee, ArrayList<String> confirmCase){
        for (int i = 1; i < datee.size(); i++) {
            String date = String.valueOf(datee.get(i));
            int confirmCasee = Integer.parseInt(confirmCase.get(i));

            XYChart.Data<String, Number> data = new XYChart.Data<String, Number>(date, confirmCasee);

            series.getData().add(data);
        }
    }

    public void setAll() throws Exception {
        chartSet(lineChart);
        addDataToChart(series, datee, confirmCase);
        lb1.setText(String.format("%s : %,d cases", graphType, Integer.parseInt(casee)));
    }
```
**Before refactor (In BarChartController.java):**
```java
    public void setAll() {
        try {
            alert.setText("");
            barChart.setTitle(countryName);

            confirmCase = gd.getCountryConfirmCase(graphType, countryName);
            showType.setValue(graphType);

            cb2.setValue(datee.get(datee.size() - 1));
            casee = confirmCase.get(confirmCase.size() - 1);

            for (int i = 1; i < datee.size(); i++) {
                XYChart.Data<String, Number> data = new XYChart.Data<String, Number>(String.valueOf(datee.get(i)), Integer.parseInt(confirmCase.get(i)));
                series.getData().add(data);
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
        lb1.setText(String.format("%s : %,d cases", graphType, Integer.parseInt(casee)));
    }
```
**After refactor (In BarChartController.java):**
```java
    public void setAll() throws Exception {
        chartSet(barChart);
        addDataToChart(series,datee,confirmCase);
        lb1.setText(String.format("%s : %,d cases", graphType, Integer.parseInt(casee)));
    }
```
## getCountryConfirmCase() method in GraphData.java
**Before refactor:**
```java
    public static ArrayList<String> getCountryConfirmCase(String type, String country) throws Exception {

        String url = "https://covid.ourworldindata.org/data/ecdc/new_deaths.csv";

        if (type.equals("Total confirmed cases")) url = "https://covid.ourworldindata.org/data/ecdc/total_cases.csv";
        else if (type.equals("Total deaths")) url = "https://covid.ourworldindata.org/data/ecdc/total_deaths.csv";
        else if (type.equals("New confirmed cases")) url = "https://covid.ourworldindata.org/data/ecdc/new_cases.csv";

        String[] c = new String[0];
        ArrayList<String> data = new ArrayList<>();
        int index = 0;

        URL oracle = new URL(url);

        //a lot of java code..
    }
```
**Refactor**: if-else statement in this method make code too long we need to extact method and remove
temp variable that make code a little bit shorter and easy to read. 

**After refactor:**
```java
    public static String selectUrl(String type) {
        String url = "https://covid.ourworldindata.org/data/ecdc/new_deaths.csv";
        if (type.equals("Total confirmed cases")){
            url = "https://covid.ourworldindata.org/data/ecdc/total_cases.csv";
        }
        else if (type.equals("Total deaths")) {
            url = "https://covid.ourworldindata.org/data/ecdc/total_deaths.csv";
        }
        else if (type.equals("New confirmed cases")) {
            url = "https://covid.ourworldindata.org/data/ecdc/new_cases.csv";
        }
        return url;
    }

    public static ArrayList<String> getCountryConfirmCase(String type, String country) throws Exception {

        String[] c = new String[0];
        ArrayList<String> data = new ArrayList<>();
        int index = 0;

        URL oracle = new URL(selectUrl(type));

        //a lot of java code..
    }
```