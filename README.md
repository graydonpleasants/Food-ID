# Food-ID
A proposal for an open food data architecture

**Author:** Graydon Pleasants

Do not republish without express consent.

# Abstract

Food waste is one of the largest environmental, nutritional and economic issues faced by today’s societies. 

“In 2010, an estimated **31 percent** or **133 billion pounds** of the 430 billion pounds of food produced was not available for human consumption at the retail and consumer levels. This amount of loss totaled an estimated $161.6 billion.(...) An estimated **141 trillion calories per year**, or **1,249 calories per capita per day**, in the food supply in 2010 went uneaten. ” [^1]

The lowest estimates indicate nearly a third of food purchased in America is wasted, with some estimates going as high as **40%**. This amounts to nearly **43 billion pounds of food wasted** by retailers, and **90 billion pounds wasted by consumers **[^1]. This project’s vision is to use tested technology already at hand in ways that can inform and combat these fundamental issues. By creating an extensible data standard for food metadata including but not limited to expiration dates, ingredients and production info, we lay the groundwork for eventual industry adoption and a new platform for innovation and product creation in the marketplace. From suggestive correction of usage habits to allergen tracking and alerts, this sets the base layer of information needed to fully solve some very real problems and user pain points. At the same time, it allows both producers and consumers to save money by more effectively tracking food as it makes its way to the plate.

# Technical Choices

At the root of this concept lies one main base requirement: a way to individually identify food items. In the current food and retail industry at large a main standard exists called **UPC**, more commonly know as the barcode. Unfortunately barcodes have some root problems that prevent their use in this type of solution. Because the UPC codes only identify the type of item, little can be done with today’s system to track an item as it makes its way to through the food chain to the eventual end consumer. 

What is also needed is a way to track an item without having to have line-of sight to the item itself, as they are usually on pallets or shelves. Thankfully a standard perfectly suited for this use already exists and has flourished in other industries: the **EPC format**. An EPC is a natural evolution of the UPC, progressing in some important ways for our use case: namely it has the space required to give each item a unique key and that it can be read at a distance, unlike the UPC which as a bar-code need line of sight. It is these capabilities that make it the natural successor to the UPC and the natural choice for this platform.

After evaluating the available tag standards, Passive RFID tags represent the best opportunity to meet the range and cost goals of the project. More research is needed to determine the best frequency of tag for this use, but UHF tags in the **865-868 MHz (Europe)** or **902-928 MHz (North America)** seem best suited and are designed with this type of use in mind.

One of the main goals of the project is to rally the industry behind a standardized version of a use-by or expiration date. Currently there is much confusion with consumers in the way products are labeled at the item level. There has been recent legislation introduced in the US to fight this: the Food Recovery Act of 2015 in Section 401 illustrates the need of having only one type of date for a consumer, but it is the opinion of this project that it doesn't go far enough to inform the user of an approaching date. 

“There has been a vast array of research on shelf life models in the past two decades.(...) Although most of the models presented (...) include factors such as humidity and atmospheric gas concentrations, they all agree on the point that temperature is the most critical factor influencing quality. ” [^4]

All research indicates the temperature an item is stored at is the most important factor in accurately calculating shelf life. By combining a standardized date format with a record of refrigeration(most likely taken per pallet or refrigeration unit) into self-annealing model for each food type, it would allow the best possible estimation of remaining time. In the future, sensor-enabled tags open up the possibility of finer per-item temperature tracking, as well as even allowing the use of gas sensors to further the accuracy of tracking decomposition. An interim solution will be needed before the full RFID system is implemented, but the expiration models generated from the finer data can also be used to more finely predict a set, printable date on a per-item basis.

As with any major technological shift, there will be costs associated with implementing the program, most significantly implementing tags in packaging and the reader system rollout. No company should be expected to make a choice that is not economically justifiable, so a main goal of the project is to make the cost of any such program low enough to be advantageous to everyone involved, from the producers to the consumer. Research indicates that reducing food waste would have a reductive effect on food pricing in the larger market, allowing the costs involved in the tagging to be some-what offset for all parties. There is also a significant business incentive for manufacturers to be involved, as **57% of consumers** think more favorably of a store that uses packaging to optimize freshness. [^2]

# Data format

By using the aforementioned EPC as an identifier for each item, we can then use this to lookup more recent information about the item. Such a lookup would be performed on an Object Name Service (ONS) that will return an IP address of the item’s data payload, much in the way a DNS does on the internet today. This allows anyone to lookup any item, which is the ideal way for the network to develop and the best way to foster innovation. Another key benefit of using the EPC network is that it is, by definition, extensible. New types of food or usage data may come in the future, so it is important to allow the item data model to be flexible enough to be extended.

> The EPCglobal Network is a computer network used to share product data between trading partners. It was created by EPCglobal. Basis for the information flow in the network is the Electronic Product Code (EPC) of each product which is stored on an RFID tag.
>
> The network manages dynamic information that is specific to variable for individual products. This includes data regarding the movement of an object throughout the product life cycle.
>
> The EPCglobal Network consists of the following components:
>
> Object Naming Service (ONS)
>
> EPC Discovery Services
>
> EPC Information Services (EPCIS)
>
> EPC Security Services
>
> The ONS is a service that enables the discovery of object information on the basis of an EPC. With the Electronic Product Code a matched URL or IP-address is searched within a data base and sent back to the requester when found. Under the URL further information about the object which is associated with the EPC can be found. The ONS is comparable to the Domain Name System which is used in the internet to translate names into IP addresses. The ONS is an instantiation of Discovery Services.
>
> The Discovery Services are an instrument to find EPC Information Services within the network. They can be compared to search engines of the internet. They offer trading partners the ability to find all parties who had possession of a given product and to share RFID events about that product.
>
> The (EPCIS) is a standard designed to enable EPC-related data sharing within and across enterprises. This data sharing is aimed to enable all network participants a common view of object information. At the EPCIS each company designated who has access to its dynamic information.
>
> The EPC Security Services are tools which allow a secured access to the information of the EPCglobal network in accordance to the access rights of the participants.

Because of the shared nature of the data, it make sense to define a centralized lookup space or an API standard for companies to follow. This level of food data has some intrinsic commercial value, so producers may be wary of contributing data to a central database that may aid a competitor. In this case, they are free to host their own ONS as long as it meets standards to interact with the rest of the network.

# Market Adoption Plan

Because of the complexities of the network, the natural approach is a 3 staged one. Firstly, it will be important to interact with stakeholders in the food production/home appliance industry to test and challenge any assumptions that have been made about the standard working for them. Also a part of this phase, a number of in market tests of the model will be required to get launch partners fully on-board with the path to adoption. From Retail or Delivery commerce experiences that prove how this will revolutionize the shopping experience for their customers, to in-fridge scanners to prove that food waste can be modeled and prevented, these will serve as a proof of the broader concept. Once these are completed successfully, we can approach the industry at large with our solution and present our findings.

One of the best parts about the proposed system is that, if implemented correctly, it should have huge benefits for both the producers and the consumers at the same time. Producers get much better inventory management, reduced out-of stock time, full food lifecycle information and a completely transformed retail experience. In the future, the groundwork is laid to allow the possibility of suggestive or predictive shopping, as well as much more detailed anonymized usage habit information. As for the consumers, once they get a reader in their home, they get access to instant inventory awareness, reminders on item status, algorithmic usage based recipes, and suggestive improvement and meal planning to name a few key benefits. 

Looking towards the future, this platform lays the groundwork for a staggering amount of possibilities, including automatic resupply of produce, self setting microwaves and ovens, automated calorie and nutrition tracking, to enhanced possibilities for food donation. Overall, this project represents a future-proof vision for the evolution of the food supply chain. By choosing to tackle this difficult task now instead of later, we further accelerate the development of ways to better utilize our finite and precious resources.

# Evaluation formula:

The idea is to create a mathematical formula, per item, to predict the quality and safety of food from 3 main inputs: an average temperature over the lifespan of the product, a general lifespan per item (generated or supplied from government data initially, but hopefully supplied by producers), and an estimated time spent out of refrigeration such as transit. These three inputs will equal PFI or the Perceived Freshness Index of each food, and this value is provided per item for applications to consume and display for users. To illustrate the concept, find sample data for items saved and wasted, in a delivery and retail context.

A linear relation has been demonstrated in many food items such as fish and other highly perishable items.

![](https://ucarecdn.com/f74a0a84-f8d5-4a1d-ad86-3e7449909fa8/)

![](https://ucarecdn.com/ea08f3c5-ee51-4287-a237-43a5766bc816/)

![](https://ucarecdn.com/91e163ea-4cba-43a3-8f90-f154c5b563d7/)

![](https://ucarecdn.com/68c561cf-c639-4d24-bb42-5487f6593dc6/)
[^6]

# Sources:

[^1]: J. Buzby, H. Wells and J. Hyman, The Estimated Amount, Value, and Calories of Postharvest Food Losses at the Retail and Consumer Levels in the United States, 1st ed. United States Department of Agriculture, 2014, p. 3.

[^2]: D. Gunders, Wasted: How America Is Losing Up to 40 Percent of Its Food from Farm to Fork to Landfill, 1st ed. Natural Resources Defense Council, 2012, pp. 1,4,7,10,12,13,17,19.

[^3]: RFID: Tracking A Tangible Approach To Gain Efficiencies In Food Retail, 1st ed. AVERY DENNISON, 2016, pp. 3,6.

[^4]: R. Jedermann, M. Nicometo, I. Uysal and W. Lang, Reducing food losses by intelligent food logistics, 1st ed. Royal Society Publishing, 2014.

[^5]: S. Vared and E. Fowler, ReFED Report 2016, 1st ed. ReFED, 2016.

[^6]: S. Vared and E. Fowler, Development of a quality index method (QIM) sensory scheme and study of shelf-life of ice-stored blackspot seabream

[^7]: M. Boeke, Kinetic Modeling of Food Quality: A Critical Review Comprehensive Reviews in Food Science and Food Safety Vol. 7, 2008
