#Conditional Formatting - Bookings: Roster List

*bold, orange (RGB 201 113 0)*

##Looks at: Booking, RSTRbkPAX1, RSTRbkPAX2

###trip departs within 90 days & roster missing info

	( Booking::d_StartLand ≥ Get ( CurrentDate ) and Booking::d_StartLand ≤ Get ( CurrentDate ) + 90 )

	and

***PAX1***

	(  IsEmpty ( RSTRbkPAX1::cd_PassportExpires )
	or IsEmpty ( RSTRbkPAX1::cs_PassportNumber )
	or IsEmpty ( RSTRbkPAX1::cs_PassportFirst )
	or IsEmpty ( RSTRbkPAX1::cs_PassportLast )
	or IsEmpty ( RSTRbkPAX1::cs_PassportNationality ) )

***PAX2***

	(  IsEmpty ( RSTRbkPAX2::cd_PassportExpires )
	or IsEmpty ( RSTRbkPAX2::cs_PassportNumber )
	or IsEmpty ( RSTRbkPAX2::cs_PassportFirst )
	or IsEmpty ( RSTRbkPAX2::cs_PassportLast )
	or IsEmpty ( RSTRbkPAX2::cs_PassportNationality ) )


###upcoming trip & any non-empty expiration less than 185 days after return

	Booking::d_StartLand ≥ Get ( CurrentDate )

	and

***PAX1***

	not IsEmpty ( RSTRbkPAX1::cd_PassportExpires )

	and RSTRbkPAX1::cd_PassportExpires ≤ Booking::cd_EndLand + 185

***PAX2***

	not IsEmpty ( RSTRbkPAX2::cd_PassportExpires )

	and RSTRbkPAX2::cd_PassportExpires ≤ Booking::cd_EndLand + 185