# Interview Experience: Wayfair - Software Engineer II (June-2024)

# [Round 2] Wayfair OnSite Virtual Round 

## Problem Statement
Given the following set of data sets, create a function that will find the coupon to display for a given category.
```java
    // {{CategoryName, CouponName}}
    String [][] coupons = {
        {"Comforter Sets", "Comforters Sale"},
        {"Bedding", "Savings on Bedding"},
        {"Bed & Bath", "Low price for Bed & Bath"}
    };
    
    // {{CategoryName, CategoryParentName}}
    String [][] categories = {
        {"Comforter Sets", "Bedding"},
        {"Bedding", "Bed & Bath"},
        {"Bed & Bath", null},
        {"Soap Dispensers", "Bathroom Accessories"},
        {"Bathroom Accessories", "Bed & Bath"},
        {"Toy Organizers", "Baby And Kids"},
        {"Baby And Kids", null}
    };
   ```
```mermaid
graph TD;
    Comforter_Sets-->Bedding;
    Bedding-->Bed_&_Bath;
    Soap_Dispensers-->Bathroom_Accessories;
    Bathroom_Accessories-->Bed_&_Bath;
    Toy_Organizers-->Baby_And_Kids;
```
| input (CategoryName) |   output (CouponName)    |
| :---:   |:------------------------:|
| Comforter Sets |     Comforters Sale      |
| Bedding |    Savings on Bedding    |
| Bathroom Accessories | Low price for Bed & Bath |
| Soap Dispensers | Low price for Bed & Bath |
| Toy Organizers |           null           |

## Requirements/Acceptance Criteria
  - Create a function that when passed a Category Name (as a String) will return Coupon Name (as a String)
  - Category structure is hierarchical. Categories without coupons inherit their parent's coupon.
  - No coupon should be returned if there are no coupons in the Category's hierarchy
    - For example: Toy Organizers receives no coupon because there is no coupon in the category hierarchy.
  - If a Category has a coupon it should not move up the hierarchy to find its Parent Category (or the Parent's Coupon)
    - For example: Comforter sets, should see the coupon for Comforter Sets and NOT Bedding
  - Beware of the following examples/edge cases and their expected behaviors:
    - Bathroom Accessories should receive the coupon for Bed & Bath because there are no coupons for Bathroom Accessories
  - Assumption: Product can only be associated with one category.

## Solution

```java
import java.util.*;

public class CategoryAndCoupons {
private final Map<String, String> couponsCategoryMap;
private final Map<String, String> parentCategoryMap;

    CategoryAndCoupons(String [][] coupons, String [][] categories) {
        this.couponsCategoryMap = new HashMap<>();
        this.parentCategoryMap = new HashMap<>();
        for (String[] coupon : coupons) this.couponsCategoryMap.put(coupon[0], coupon[1]);
        for (String[] category : categories) this.parentCategoryMap.put(category[0], category[1]);
    }

    private String getCoupon(String categoryName) {
        if(this.couponsCategoryMap.containsKey(categoryName))
            return this.couponsCategoryMap.get(categoryName);
        if(Objects.isNull(this.parentCategoryMap.get(categoryName)))
            return null;
        return getCoupon(this.parentCategoryMap.get(categoryName));
    }

    public static void main(String[] args) {
        String [][] coupons = {
                {"Comforter Sets", "Comforter Sale"},
                {"Bedding", "Savings on Bedding"},
                {"Bed & Bath", "Low price for Bed & Bath"}
        };
        // Bed & Bath > Bedding > Comforter Sets
        // Bed & Bath > Bathroom Accessories > Soap Dispensers
        // Baby And Kids > Toy Organizers
        String [][] categories = {
                {"Comforter Sets", "Bedding"},
                {"Bedding", "Bed & Bath"},
                {"Bed & Bath", null},
                {"Soap Dispensers", "Bathroom Accessories"},
                {"Bathroom Accessories", "Bed & Bath"},
                {"Toy Organizers", "Baby And Kids"},
                {"Baby And Kids", null}
        };
        CategoryAndCoupons categoryAndCoupons = new CategoryAndCoupons(coupons, categories);
        System.out.println("Comforter Sets => " + categoryAndCoupons.getCoupon("Comforter Sets"));
        System.out.println("Bedding => " + categoryAndCoupons.getCoupon("Bedding"));
        System.out.println("Bathroom Accessories => " + categoryAndCoupons.getCoupon("Bathroom Accessories"));
        System.out.println("Soap Dispensers => " + categoryAndCoupons.getCoupon("Soap Dispensers"));
        System.out.println("Toy Organizers => " + categoryAndCoupons.getCoupon("Toy Organizers"));
    }
}
```
## Problem Statement (Follow up 1)
The system has added a new piece of data to the coupon - "Date Modified". Use this when resolving any ties (when 1 Category
has 2+ Coupons).
```java
    // {{CategoryName, CouponName, DateModified}}
    String [][] coupons = {
            {"Comforter Sets", "Comforter Sale", "2020-01-01"},
            {"Comforter Sets", "Cozy Comforter Coupon", "2020-01-01"},
            {"Bedding", "Best Bedding Bargains", "2019-01-01"},
            {"Bedding", "Savings on Bedding", "2019-01-01"},
            {"Bed & Bath", "Low price for Bed & Bath", "2018-01-01"},
            {"Bed & Bath", "Bed & Bath extravaganza", "2019-01-01"},
            {"Bed & Bath", "Big Savings for Bed & Bath", "2030-01-01"}
    };
    
    // {{CategoryName, CategoryParentName}}
    String [][] categories = {
        {"Comforter Sets", "Bedding"},
        {"Bedding", "Bed & Bath"},
        {"Bed & Bath", null},
        {"Soap Dispensers", "Bathroom Accessories"},
        {"Bathroom Accessories", "Bed & Bath"},
        {"Toy Organizers", "Baby And Kids"},
        {"Baby And Kids", null}
    };
   ```
```mermaid
graph TD;
    Comforter_Sets-->Bedding;
    Bedding-->Bed_&_Bath;
    Soap_Dispensers-->Bathroom_Accessories;
    Bathroom_Accessories-->Bed_&_Bath;
    Toy_Organizers-->Baby_And_Kids;
```
| input (CategoryName) |             output (CouponName)             |
| :---:   |:-------------------------------------------:|
| Bed & Bath |           Bed & Bath extravaganza           |
| Bedding | Best Bedding Bargains \| Savings on Bedding |
| Bathroom Accessories |           Bed & Bath extravaganza           |
| Comforter Sets |   Comforter Sale \| Cozy Comforter Coupon   |

## Requirements/Acceptance Criteria
- Create a function that when passed a Category Name (as a String) will return one Coupon Name (as a String)
- If a Category has more than 1 coupon the Coupon with the most recent DateModified should be returned
- If a Coupon's DateModified is in the future, it should not be returned
- Category structure is hierarchical. Categories without coupons inherit their parent's coupon.

## Additional Guidance
- Use of the internet for syntax and basic implementation questions is allowed.
- Think out loud! Describe your approach and discuss changes as new information surfaces

## Solution

```java
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.*;
import java.util.stream.Collectors;

public class CategoryAndCouponsF1 {
    private final String datePattern = "yyyy-MM-dd";
    private final DateTimeFormatter FORMATTER = DateTimeFormatter.ofPattern(datePattern);

    @Setter
    @Getter
    @ToString
    static class Coupon {
        private String couponName;
        private LocalDate dateModified;
        public Coupon (String couponName, LocalDate dateModified) {
            this.couponName = couponName;
            this.dateModified = dateModified;
        }
    }

    private final Map<String, Map<LocalDate, List<Coupon>>> categoryCouponsMapGroupByModifiedDate;
    private final Map<String, String> parentCategoryMap;

    CategoryAndCouponsF1(String [][] coupons, String [][] categories) {
        Map<String, List<Coupon>> couponsCategoryMap = new HashMap<>();
        this.parentCategoryMap = new HashMap<>();
        this.categoryCouponsMapGroupByModifiedDate = new HashMap<>();
        for (String[] coupon : coupons) {
            if(!couponsCategoryMap.containsKey((coupon[0])))
                couponsCategoryMap.put(coupon[0], new ArrayList<>());

            LocalDate dateModified = LocalDate.parse(coupon[2], FORMATTER);
            if(dateModified.isBefore(LocalDate.now()))
                couponsCategoryMap.get(coupon[0]).add(new Coupon(coupon[1], dateModified));
        }
        for (String[] category : categories) this.parentCategoryMap.put(category[0], category[1]);
        for (Map.Entry<String, List<Coupon>> couponsEntry : couponsCategoryMap.entrySet()) {
            this.categoryCouponsMapGroupByModifiedDate.put(couponsEntry.getKey(), couponsEntry.getValue().stream().collect(Collectors.groupingBy(Coupon::getDateModified)));
        }
    }

    private String getCoupon(String categoryName) {
        if(this.categoryCouponsMapGroupByModifiedDate.containsKey(categoryName)
                && this.categoryCouponsMapGroupByModifiedDate.get(categoryName).values().stream().findFirst().isPresent())
            return this.categoryCouponsMapGroupByModifiedDate.get(categoryName).values().stream().findFirst().get().stream().map(Coupon::getCouponName).collect(Collectors.joining(" | "));
        if(Objects.isNull(this.parentCategoryMap.get(categoryName)))
            return null;
        return getCoupon(this.parentCategoryMap.get(categoryName));
    }

    public static void main(String[] args) {
        String [][] coupons = {
                {"Comforter Sets", "Comforter Sale", "2020-01-01"},
                {"Comforter Sets", "Cozy Comforter Coupon", "2020-01-01"},
                {"Bedding", "Best Bedding Bargains", "2019-01-01"},
                {"Bedding", "Savings on Bedding", "2019-01-01"},
                {"Bed & Bath", "Low price for Bed & Bath", "2018-01-01"},
                {"Bed & Bath", "Bed & Bath extravaganza", "2019-01-01"},
                {"Bed & Bath", "Big Savings for Bed & Bath", "2030-01-01"}
        };
        // Bed & Bath > Bedding > Comforter Sets
        // Bed & Bath > Bathroom Accessories > Soap Dispensers
        // Baby And Kids > Toy Organizers
        String [][] categories = {
                {"Comforter Sets", "Bedding"},
                {"Bedding", "Bed & Bath"},
                {"Bed & Bath", null},
                {"Soap Dispensers", "Bathroom Accessories"},
                {"Bathroom Accessories", "Bed & Bath"},
                {"Toy Organizers", "Baby And Kids"},
                {"Baby And Kids", null}
        };
        CategoryAndCouponsF1 categoryAndCoupons = new CategoryAndCouponsF1(coupons, categories);

        System.out.println("Bed & Bath => " + categoryAndCoupons.getCoupon("Bed & Bath"));
        System.out.println("Bedding => " + categoryAndCoupons.getCoupon("Bedding"));
        System.out.println("Bathroom Accessories => " + categoryAndCoupons.getCoupon("Bathroom Accessories"));
        System.out.println("Comforter Sets => " + categoryAndCoupons.getCoupon("Comforter Sets"));
    }
}
```
## Problem Statement (Follow up 2)
Now that we know what coupon to show to the user, let's make sure the user can apply the coupon by changing the selling price
of a Product.<br>
**_Format (Category Name, Coupon Name, Date Modified, Discount)_**
```java
    // {{CategoryName, CouponName, DateModified, Discount}}
    String [][] coupons = {
            {"Comforter Sets", "Comforters Sale", "2020-01-01", "10%"},
            {"Comforter Sets", "Cozy Comforter Coupon", "2020-01-01", "$15"},
            {"Bedding", "Best Bedding Bargains", "2019-01-01", "35%"},
            {"Bedding", "Savings on Bedding", "2019-01-01", "25%"},
            {"Bed & Bath", "Low price for Bed & Bath", "2018-01-01", "50%"},
            {"Bed & Bath", "Bed & Bath extravaganza", "2019-01-01", "75%"}
    };
    
    // {{CategoryName, CategoryParentName}}
    String [][] categories = {
        {"Comforter Sets", "Bedding"},
        {"Bedding", "Bed & Bath"},
        {"Bed & Bath", null},
        {"Soap Dispensers", "Bathroom Accessories"},
        {"Bathroom Accessories", "Bed & Bath"},
        {"Toy Organizers", "Baby And Kids"},
        {"Baby And Kids", null}
    };

    // {{ProductName, Price, CategoryName}}
    String [][] products = {
        {"Cozy Comforter Sets", "100.00", "Comforter Sets"},
        {"All-in-one Bedding Set", "50.00", "Bedding"},
        {"Infinite Soap Dispenser", "500.00", "Bathroom Accessories"},
        {"Rainbow Toy Box", "257.00", "Baby And Kids"}
    };
   ```
```mermaid
graph TD;
    Comforter_Sets-->Bedding;
    Bedding-->Bed_&_Bath;
    Soap_Dispensers-->Bathroom_Accessories;
    Bathroom_Accessories-->Bed_&_Bath;
    Toy_Organizers-->Baby_And_Kids;
```
| input (CategoryName) |           output (Discount)           |
| :---:   |:---------------------------------------:|
| Comforter Sets |90.0 OR 85.0|
| Bedding |32.5 OR 37.5|
| Bathroom Accessories |125.0|
|Baby And Kid|257.0|

## Requirements/Acceptance Criteria
- Function takes a String representing the Product Name
- Function Returns the discounted price (Product price minus the coupon discount) as a String

## Solution

```java
import lombok.Getter;
import lombok.Setter;
import lombok.ToString;

import java.time.LocalDate;
import java.time.format.DateTimeFormatter;
import java.util.*;
import java.util.stream.Collectors;

//https://leetcode.com/discuss/interview-question/4491846/Wayfair-or-SDE2-L2-4-Onsite-Virtual-or-Coupon-category/
//https://leetcode.com/discuss/interview-question/4799068/Wayfair-or-Onsite-or-Round-1-DSA-or-Bangalore/

public class CategoryAndCouponsF2 {
    private final String datePattern = "yyyy-MM-dd";
    private final DateTimeFormatter FORMATTER = DateTimeFormatter.ofPattern(datePattern);

    @Setter
    @Getter
    @ToString
    static class Coupon {
        private String couponName;
        private LocalDate dateModified;
        private String discount;
        public Coupon(String couponName, LocalDate dateModified, String discount) {
            this.couponName = couponName;
            this.dateModified = dateModified;
            this.discount = discount;
        }
    }

    @Setter
    @Getter
    @ToString
    static class Product {
        private String name;
        private String categoryName;
        private Double price;
        public Product(String categoryName, String name, Double price) {
            this.categoryName = categoryName;
            this.name = name;
            this.price = price;
        }
    }
    private final Map<String, String> couponsDiscountMap;
    private final Map<String, String> parentCategoryMap;
    private final Map<String, Product> productCategoryMap;
    private final Map<String, Map<LocalDate, List<CategoryAndCouponsF2.Coupon>>> categoryCouponsMapGroupByModifiedDate;

    CategoryAndCouponsF2(String [][] coupons, String [][] categories, String [][] products) {
        Map<String, List<Coupon>> couponsCategoryMap = new HashMap<>();
        this.parentCategoryMap = new HashMap<>();
        this.productCategoryMap = new HashMap<>();
        this.couponsDiscountMap = new HashMap<>();
        this.categoryCouponsMapGroupByModifiedDate = new HashMap<>();
        for (String[] coupon : coupons) {
            if(!couponsCategoryMap.containsKey((coupon[0])))
                couponsCategoryMap.put(coupon[0], new ArrayList<>());

            LocalDate dateModified = LocalDate.parse(coupon[2], FORMATTER);

            if(dateModified.isBefore(LocalDate.now()))
                couponsCategoryMap.get(coupon[0]).add(new Coupon(coupon[1], dateModified, coupon[3]));

            couponsDiscountMap.put(coupon[1], coupon[3]);
        }
        for (String[] category : categories) this.parentCategoryMap.put(category[0], category[1]);
        for (Map.Entry<String, List<CategoryAndCouponsF2.Coupon>> couponsEntry : couponsCategoryMap.entrySet())
            this.categoryCouponsMapGroupByModifiedDate.put(couponsEntry.getKey(), couponsEntry.getValue().stream().collect(Collectors.groupingBy(CategoryAndCouponsF2.Coupon::getDateModified)));
        for (String[] product : products) this.productCategoryMap.put(product[0], new Product(product[2], product[0], Double.parseDouble(product[1])));
    }

    private Set<String> getCoupon(String categoryName) {
        if(this.categoryCouponsMapGroupByModifiedDate.containsKey(categoryName)
                && this.categoryCouponsMapGroupByModifiedDate.get(categoryName).values().stream().findFirst().isPresent())
            return this.categoryCouponsMapGroupByModifiedDate.get(categoryName).values().stream().findFirst().get().stream().map(CategoryAndCouponsF2.Coupon::getCouponName).collect(Collectors.toSet());
        if(Objects.isNull(this.parentCategoryMap.get(categoryName)))
            return null;
        return getCoupon(this.parentCategoryMap.get(categoryName));
    }

    private  String displayDiscountPrice(String productName) {
        Product product = this.productCategoryMap.get(productName);
        Set<String> coupon = getCoupon(product.getCategoryName());
        if(Objects.isNull(coupon))
            return product.getPrice().toString();
        return String.join(" OR ", coupon.stream()
                .map(s -> applyDiscount(product.getPrice(), this.couponsDiscountMap.get(s.trim())).toString())
                .collect(Collectors.toSet())
        );
    }

    private Double applyDiscount(Double originalPrice, String discount) {
        double discountAmount;
        if(discount.contains("$"))
            discountAmount = Double.parseDouble(discount.replaceAll("\\$", ""));
        else
            discountAmount = originalPrice * (Double.parseDouble(discount.replaceAll("%", "")) / 100);
        return originalPrice - discountAmount;
    }

    public static void main(String[] args) {
        String [][] coupons = {
                {"Comforter Sets", "Comforters Sale", "2020-01-01", "10%"},
                {"Comforter Sets", "Cozy Comforter Coupon", "2020-01-01", "$15"},
                {"Bedding", "Best Bedding Bargains", "2019-01-01", "35%"},
                {"Bedding", "Savings on Bedding", "2019-01-01", "25%"},
                {"Bed & Bath", "Low price for Bed & Bath", "2018-01-01", "50%"},
                {"Bed & Bath", "Bed & Bath extravaganza", "2019-01-01", "75%"}
        };
        // Bed & Bath > Bedding > Comforter Sets
        // Bed & Bath > Bathroom Accessories > Soap Dispensers
        // Baby And Kids > Toy Organizers
        String [][] categories = {
                {"Comforter Sets", "Bedding"},
                {"Bedding", "Bed & Bath"},
                {"Bed & Bath", null},
                {"Soap Dispensers", "Bathroom Accessories"},
                {"Bathroom Accessories", "Bed & Bath"},
                {"Toy Organizers", "Baby And Kids"},
                {"Baby And Kids", null}
        };

        String [][] products = {
                {"Cozy Comforter Sets", "100.00", "Comforter Sets"},
                {"All-in-one Bedding Set", "50.00", "Bedding"},
                {"Infinite Soap Dispenser", "500.00", "Bathroom Accessories"},
                {"Rainbow Toy Box", "257.00", "Baby And Kids"}
        };

        CategoryAndCouponsF2 cc = new CategoryAndCouponsF2(coupons, categories, products);

        System.out.println("Comforter Sets => " + cc.displayDiscountPrice("Cozy Comforter Sets"));
        System.out.println("Bedding => " + cc.displayDiscountPrice("All-in-one Bedding Set"));
        System.out.println("Bathroom Accessories => " + cc.displayDiscountPrice("Infinite Soap Dispenser"));
        System.out.println("Baby And Kids => " + cc.displayDiscountPrice("Rainbow Toy Box"));
    }
}
```
