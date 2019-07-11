#Getting GPS Coordinates for points within a survey area with known corners

The problem: I have a 10m x 10m survey plot where I know the lat&long of the corners (0,0;0,10;10,10;10,0) and XY coords of coolonies within the 10x10m plot. How do I get lat&long for each colony in the plot?

My solution:
From Rudnicki, M., Meyer, T. H., Rudnicki, M., & Meyer, T. H. (2007). systems Methods to Convert Local Sampling Coordinates Coordinate Systems. - "The Coons patch surface is parameterized by two logical variables, u and v. These variables essentially define the x and y directions as defined by the edges of the plot; note that they have no a priori relationship to east or north.Both u and v can have values in the range [0, 1]. We denote the Coons patch as s(u, v), meaning that the function s evaluated at parameter values u,v yields a point within the patch. The four corner points of the plot are denoted p00, p01, p10, and p11. With this notion, the formula for a Coons patch is s(u,v)=((1-u)*(1-v)*P00) + ((1-u)*v*P01) + (u*v*P11) + (u*(1-v)*P10)."



```{r}
GPSFromXY_100m2Plot<-function(P00,P01,P11,P10,x,y){
  #function uses coons patch geometry to solve for the lat/long of a point on an XY grid where the lat/long of the corners of the XY grid are known. Works for a 10mx10m survey plot.
  #
  options(digits = 10) #need higher resolution
  #P00, P01, P11,P10 must be lat long coords of the corners of a square survey plot
  #function returns the lat long for the xy point in question
  # XY have to be on 0-1 scale, so divide by 10
  x<-x/10
  y<-y/10
  lat<-((1-x)*(1-y)*P00[1]) + ((1-x)*y*P01[1]) + (x*y*P11[1]) + (x*(1-y)*P10[1])
  long<-((1-x)*(1-y)*P00[2]) + ((1-x)*y*P01[2]) + (x*y*P11[2]) + (x*(1-y)*P10[2])
  return(c(lat,long))

}

#### Example
p1<-c(24.560809,	-81.50137)
p2<-c(24.560809,	-81.501272)
p3<-c(24.560718	,-81.501272)
p4<-c(24.560718	,-81.501370)

GPSFromXY_100m2Plot(p1,p2,p3,p4,5,5)
```