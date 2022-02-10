a = 0; % lower limit
b = 2*pi/3; % upper limit
interval = 10;
disp(integral(a,b,interval));

function returnlist = integral(a,b,interval)
    returnlist = [];
    low = a;   up = b; 
    for v = 1:6  % decides 3 rules
        result = []*interval;  
        for i = 1:interval
            % interval list
            in = (low)*(i+1); % list of size i+1
            for k = 2:i; in(k) = (b*(k-1))/i; end % midpoints
            in(i+1) = up;    % upper limit at last index 
            total = 0;          % resetting for next iteration
            for j  = 1:i
                a = in(j);  b = in(j+1);
                if v == 1 % Trapeziodal rule
                    total = total + ( (sine(a) + sine(b)) /2) * (b-a);  
                elseif v == 2 % Simpson 1/3 rule
                    total = total + ( (sine(a) + 4*sine((a+b)/2) + sine(b)) /3) * ((b-a)/2);
                elseif v == 3 % Simpson 3/8 rule
                     total = total + ( (sine(a) + 3*sine(a+((b-a)/3)) + 3*sine(b-((b-a)/3)) + sine(b)) /4) * ((b-a)/2);
                elseif v == 4 % Trapeziodal rule
                     total = total + ( (cosine(a) + cosine(b)) /2) * (b-a);
                elseif v == 5 % Simpson 1/3 rule
                     total = total + ( (cosine(a) + 4*cosine((a+b)/2) + cosine(b)) /3) * ((b-a)/2);
                 elseif v == 6 % Simpson 3/8 rule
                     total = total + ( (cosine(a) + 3*cosine(a + ((b-a)/3)) + 3*cosine(b - ((b-a)/3)) + cosine(b)) /4) * ((b-a)/2);
                end  
            end
            result(i)= total;        
        end
    disp(result);
    returnlist = [ returnlist result(end) ]; % last value
    figure
    plot(1:interval,result);
    xlabel('Number of integration intervals'); ylabel('Intergral Value');         
    if v == 1;     title('Trapeziodal rule - sin');
    elseif v == 2; title('Simpson 1/3 rule - sin');
    elseif v == 3; title('Simpson 3/8 rule - sin');
    elseif v == 4; title('Trapeziodal rule - cos');
    elseif v == 5; title('Simpson 1/3 rule - cos');
    elseif v == 6; title('Simpson 3/8 rule - cos');
    end
    end
end


function sinval = sine(x)
    terms = 15;
    sinans = []*terms;
    sinans(1) = x; % first term = x
    for j = 3:2:(terms-1)*2+1 % j = 3 to 39
        % (j-1)/2 makes 3,5,7 as 1,2,3 ,so power will be odd,even,odd 
        p = (j-1) / 2;
        % sum of all previous values + (-1^(j-1)/2) * (x^j)/j!
        sinans(p+1) = sinans(p) + ((-1)^p)*((x^j)/factorial(j));
    end
    sinval = sinans(converge(sinans));
end

function cosval = cosine(x)
    terms = 15;
    cosans = []*terms;
    cosans(1) = 1;  % first term = 1
    for j = 2:2:(terms-1)*2
        % j/2 makes 2,4,6 as 1,2,3 ,so power will be odd,even,odd
        p = j / 2;
        % sum of all previous values + (-1^j/2) * (x^j)/j!
        cosans(p+1) = cosans(p) + ((-1)^p)*((x^j)/factorial(j));
    end
    cosval = cosans(converge(cosans));
end   

function convans = converge(series)
    for i = 1 : length(series)-2
        if  series(i+2)-series(i+1) == series(i+1)-series(i)
            % if next - current  = current - prev
            convans = i;  break;
        else
            convans = i+2; % else last term(no. of terms)
        end
    end
end
