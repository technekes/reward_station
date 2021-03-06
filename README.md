# Xceleration Reward Station

Client library for Xceleration rewardstation.com SOAP service

## Basic Usage

Common scenario is creating client instance using required parameters `:client_id` and `:client_password`.
Client implements several methods for accessing reward station SOAP API.

### Initialization

    reward_station = RewardStation::Client.new {
        :client_id => "112112",                      # required
        :client_password => "fsdftr#",               # required
        :organization_id => '150',                   # optional, default Organization ID
        :program_id => 25,                           # optional, default Program ID
        :point_reasond_code_id => 129                # optional, default Point Reason Code ID
        :token => "sdfweqwrtwerfasdfas"              # optional, initial Access Token value
        :new_token_callback => lambda{ |token| ... } # optional, callback on Access Token change
    }

### New Token Callback
You can specify callback in constructor as `lambda` or `proc`
Or you can specify callback as block:

    reward_station.new_token_calback do |new_token|
        # notify other client instance about new token
    end

### Return Token
Access token request. Usually not needed on businnes level because client requests it when it became invalid or expired

    token = reward_station.return_token

### Award Points
Update award points

    user_id = "130"
    points = 10
    description = "Action 'Call to client' "
    program_id = 90 # optional
    point_reasond_code_id = 129 # optional

    confirmation_number = reward_station.award_points user_id, points, description, program_id, point_reason_code_id

### Create User

    user_attributes = reward_station.create_user :organization_id => '150',
                                                 :email => 'john5@company.com',
                                                 :first_name => 'John',
                                                 :last_name => 'Smith',
                                                 :user_name => 'john5@company.com',
                                                 :balance => 0
    puts user_attributes.inspect
    # {
    #    :user_id => '6727',
    #    :client_id => '100080',
    #    :user_name => 'john5@company.com',
    #    :email => 'john5@company.com',
    #    :encrypted_password => nil,
    #    :first_name => 'John',
    #    :last_name => 'Smith',
    #    :address_one => nil,
    #    :address_two => nil,
    #    :city => nil,
    #    :state_code => nil,
    #    :province => nil,
    #    :postal_code => nil,
    #    :country_code => 'USA',
    #    :phone => nil,
    #    :organization_id => '150',
    #    :organization_name => nil,
    #    :rep_type_id => '0',
    #    :client_region_id => '0',
    #
    #    :is_active => true,
    #    :point_balance => '0',
    #    :manager_id => '0',
    #    :error_message => nil
    # }

### Update User

Similar to 'Create User'

    user_attributes = reward_station.update_user user_id, :organization_id => '150',
                                                          :email => 'john5@company.com',
                                                          :first_name => 'John',
                                                          :last_name => 'Smith',
                                                          :user_name => 'john5@company.com',
                                                          :balance => 0


### Return Points Summary

Returns user points. Required parameter - User ID

    user_id = 130

    summary = service.return_point_summary user_id

    puts summary.inspect

    # {
    #   :user_id => '577',
    #   :is_active => true,
    #   :points_earned => '465',
    #   :points_redeemed => '0',
    #   :points_credited => '0',
    #   :points_balance => '465'
    # }


### Return Popular Products

Returns an array of popular products for user

    user_id = 130

    popular_products = service.return_popular_products user_id

    puts popular_products.inspect

    # [
    #   {
    #    :product_id => 'MC770LLA',
    #    :name => 'iPad 2 with Wifi - 32GB',
    #    :description => 'The NEW Apple iPad 2 - Thinner, lighter, and full of great ideas.  Once you pick up iPad 2, it’ll be hard to put down.  That’s the idea behind the all-new design. It’s 33 percent thinner and up to 15 percent lighter, so it feels even more comfortable in your hands.  And, it makes surfing the web, checking email, watching movies, and reading books so natural, you might forget there’s incredible technology under your fingers.<br><br><b>Dual-core A5 chip</b>.<br> Two powerful cores in one A5 chip mean iPad can do twice the work at once.  You’ll notice the difference when you’re surfing the web, watching movies, making FaceTime video calls, gaming, and going from app to app to app.  Multitasking is smoother, apps load faster, and everything just works better.<br><br><b>Superfast graphics</b>. <br>With up to nine times the graphics performance, gameplay on iPad is even smoother and more realistic.  And faster graphics help apps perform better — especially those with video.  You’ll see it when you’re scrolling through your photo library, editing video with iMovie, and viewing animations in Keynote.<br><br><b>Battery life keeps on going. So you can, too.</b><br> Even with the new thinner and lighter design, iPad has the same amazing 10-hour battery life.  That’s enough juice for one flight across the ocean, or one movie-watching all-nighter, or a week’s commute across town.  The power-efficient A5 chip and iOS keep battery life from fading away, so you can get carried away.<br><br><b>Two cameras.</b><br> You’ll see two cameras on iPad — one on the front and one on the back.  They may be tiny, but they’re a big deal.  They’re designed for FaceTime video calling, and they work together so you can talk to your favorite people and see them smile and laugh back at you. The front camera puts you and your friend face-to-face.  Switch to the back camera during your video call to share where you are, who you’re with, or what’s going on around you. When you’re not using FaceTime, let the back camera roll if you see something movie-worthy. It’s HD, so whatever you shoot is a mini-masterpiece. And you can take wacky snapshots in Photo Booth. It’s the most fun a face can have.<br><br><b>Due to the demand for this item, please allow up to 8-10 weeks for delivery</b>.',
    #    :points => '10927',
    #    :category => 'Office & Computer',
    #    :manufacturer => 'Apple',
    #    :small_image_url => 'https://www.rewardstation.com/catalogimages/MC769LLA.gif',
    #    :large_image_url => 'https://www.rewardstation.com/catalogimages/MC769LLA.jpg'
    #   }, {
    #   ...
    #   }
    # ]


## Client Stub

Client supports stub mode.
Client stub supports all request methods supported by `RewardStation::Client` but it doesn't make requests to Reward Station API. It just returns predefined SOAP responses.
You can override those responses in `config/reward_station/responses` folder. For example if `award_points` method response should be overridden then add `award_points.xml` file to `config/reward_station/responses` folder.

### Stub Initialization

    stub = RewardStation::Client.stub

### config/reward_station/responses/award_points.xml

    <soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
       <soap:Body>
          <AwardPointsResponse xmlns="http://rswebservices.rewardstation.com/">
             <AwardPointsResult>
                <UserID>577</UserID>
                <Points>10</Points>
                <ConfirmationNumber>9376</ConfirmationNumber>
                <ErrorMessage/>
             </AwardPointsResult>
          </AwardPointsResponse>
       </soap:Body>
    </soap:Envelope>


## Single-Sign-On

Basic SSO logic implemented in SAML::AuthResponse class. Example usage of AuthResponse:

### SessionController

    require 'saml/auth_response'

    class SessionController < ApplicationController

      def create
        @user_session = UserSession.new(params[:user_session])
        if @user_session.save
            if session[:saml_request].present?
                sso_params(session[:saml_request], session[:relay_state], current_user)
                session[:saml_request] = session[:relay_state] = nil
                render :template => "session/signon"
                return
             ...
            end
          ...
        end
      end

      def signon
        if current_user.present?
          sso_params(params[:SAMLRequest], params[:RelayState], current_user)

          render :template => "session/signon"
        else
          session[:saml_request] = params[:SAMLRequest]
          session[:relay_state] = params[:RelayState]
          redirect_to signin_url
        end
      end

      def destroy
        current_user_session.destroy
        redirect_to signin_url
      end

      protected

      def sso_params saml_request, relay_state, user
        @saml_response = SAML::AuthResponse.new(saml_request).response_url(user.xceleration_id)
        @relay_state = relay_state
      end
    end

### signon.html.erb

    <html>
      <body>
        <form  id="sso_form" action="http://www6.rewardstation.net/sso/100080/AssertionService.aspx?binding=urn:oasis:names:tc:SAML:2.0:bindings:HTTP-POST" method="post">
          <input type="hidden" name="SAMLResponse" value="<%= @saml_response %>"/>
          <input type="hidden" name="RelayState" value="<%= @relay_state %>"/>
        </form>
        <script type="text/javascript">
          document.getElementById('sso_form').submit();
        </script>
      </body>
    </html>

