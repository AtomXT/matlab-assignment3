function draw_circles
figure('WindowButtonDownFcn',@wbdcb,'WindowButtonMotionFcn',@wbmcb,...
    'WindowButtonUpFcn',@done);
ah = axes('SortMethod','childorder');
circles = [rectangle('Position', [0 0 0 0], 'Curvature', 1), ...
    rectangle('Position', [0 0 0 0], 'Curvature', 1)];%Creat two circles in the figure.
axis equal;
axis ([0 1 0 1]);

circle_count = 1;
error = 0.001;%Use when judging two circles tangent.
running = 0;%Makesure nothing happens before clicking the left button.
point1=[0,0];%Global Variablies
point2=[0,0];
circle(circle_count,:)=[0,0,0,0];
   function wbdcb(src,~)%Use '~' to substitute variables that won't be used.
      if strcmp(get(src,'SelectionType'),'normal')
         running = 1;%Start after clicking the left mouse button.
         [x,y] = get_point(ah);
         point1 = [x,y];
         if circle_count == 1%Check the number of circles when click the left button.
            set(circles, 'Position', [0 0 0 0]);%Clear old circles.
            delete(findobj('LineStyle','--'));%Clear old common tangents.
         end
      end
   end
  function wbmcb(~,~)
      if running
         [xn,yn] = get_point(ah);
         point2 = [xn,yn];
         d = norm(point1-point2);%the distance between two points.
         center = (point1+point2)/2;
         set(circles(circle_count), 'Position', [center-d/2, d, d]);
         axis ([0 1 0 1]);
      end
  end
       function done(~,~)
           running = 0;%Close the function after releasing the left mouse button.
           circle(circle_count,:)=[point1,point2];
           circle_count = circle_count + 1;
           if circle_count > 2
               circle_count = 1;
               drawCommonTangent(circle(1,:),circle(2,:));%Draw the common tangent of two cirlces.
               axis([0 1 0 1]);
       end
   end
   function [x,y] = get_point(ah)
      cp = get(ah,'CurrentPoint');  
      x = cp(1,1);y = cp(1,2); 
   end
    function drawCommonTangent(circle1,circle2)
        center1 = [(circle1(1)+circle1(3))/2,(circle1(2)+circle1(4))/2];
        center2 = [(circle2(1)+circle2(3))/2,(circle2(2)+circle2(4))/2];
        r1 = norm(circle1(1:2)-circle1(3:4))/2;
        r2 = norm(circle2(1:2)-circle2(3:4))/2;%The center and radius of two circles.
        CenterDistance = norm(center1-center2);%The distance of two centers.
        Theta = asin((r1+r2)/CenterDistance);%The angle between the two center lines and an external common tangent
        Theta1 = asin((r2-r1)/CenterDistance);%The angle between the two center lines and an internal common tangent
        O = [(r1/(r1+r2)*(center2(1)-center1(1)) + center1(1)),...
            (r1/(r1+r2)*(center2(2)-center1(2))+center1(2))];%Intersection of the external common tangent.
        if r1>=r2;
            O1 = [(r2/abs(r1-r2)*(center2(1)-center1(1)) + center2(1)),...
                (r2/abs(r1-r2)*(center2(2)-center1(2)) +center2(2))];%Intersection of the internal common tangent.
        else
            O1 = [(r1/abs(r1-r2)*(center1(1)-center2(1)) + center1(1)),...
                (r1/abs(r1-r2)*(center1(2)-center2(2)) +center1(2))];%Intersection of the internal common tangent.
        end
        O3 = [r1*(center2(1)-center1(1))/(r1+r2)+center1(1),...
            r1*(center2(2)-center1(2))/(r1+r2)+center1(2)];%The tangent point of the two circles.
        h = hgtransform;
        h1 = hgtransform;
        h2 = hgtransform;
        h3 = hgtransform;
        h4 = hgtransform;
        ct = line([center1(1),center2(1)],[center1(2),center2(2)],...
            'Parent',h,'LineStyle','--');
        ct1 = line([center1(1),center2(1)],[center1(2),center2(2)],...
            'Parent',h1,'LineStyle','--');
        ct2 = line([center1(1),center2(1)],[center1(2),center2(2)],...
            'Parent',h2,'LineStyle','--');
        ct3 = line([center1(1),center2(1)],[center1(2),center2(2)],...
            'Parent',h3,'LineStyle','--');
        ct4 = line([center1(1),center2(1)],[center1(2),center2(2)],...
            'Parent',h4,'LineStyle','--');
        PI = makehgtform('translate',-[O(1) O(2) 0]);
        PE = makehgtform('translate',-[O1(1) O1(2) 0]);
        R = makehgtform('zrotate',Theta);
        R1 = makehgtform('zrotate',-Theta);
        R2 = makehgtform('zrotate',Theta1);
        R3 = makehgtform('zrotate',-Theta1);
        BI = makehgtform('translate',[O(1) O(2) 0]);
        BE = makehgtform('translate',[O1(1) O1(2) 0]);
        P = makehgtform('translate',-[O3(1) O3(2) 0]);
        R4 = makehgtform('zrotate',pi/2);
        B = makehgtform('translate',[O3(1) O3(2) 0]);
        PN = makehgtform('translate',-[circle2(1) circle2(2) 0]);
        BN = makehgtform('translate',[circle2(1) circle2(2) 0]);
        S = makehgtform('scale',2);
        if CenterDistance > r1+r2+error
            set(h,'Matrix',BI*R*PI);
            set(h1,'Matrix',BI*R1*PI);
            set(h2,'Matrix',BE*R2*PE);
            set(h3,'Matrix',BE*R3*PE);
        elseif r1+r2-error < CenterDistance && CenterDistance <=r1+r2+error
            set(h2,'Matrix',BE*R2*PE);
            set(h3,'Matrix',BE*R3*PE);
            set(h4,'Matrix',B*R4*P);
        elseif abs(r1-CenterDistance) <= r2-error
            set(h2,'Matrix',BE*R2*PE);
            set(h3,'Matrix',BE*R3*PE);
        elseif r2-error < abs(r1-CenterDistance) && abs(r1-CenterDistance) <= r2+error;
            set(h4,'Matrix',BN*R4*S*PN);
        else
            delete(findobj('LineStyle','--'));%NO tangents. So delete all lines. 
        end
    end
end
