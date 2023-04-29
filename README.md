# Credit Valuation Adjustment (CVA) Introduction

Credit valuation adjustment (CVA) is the market price of counterparty credit risk that has become a central part of counterparty credit risk management.  By definition, CVA is the difference between the risk-free portfolio value and the true/risky portfolio value. In practice, CVA should be computed at portfolio level. That means calculation should take Master agreement and CSA agreement into account. 

CVA not only allows institutions to quantify counterparty risk as a single measurable P&L number, but also offers an opportunity for banks to dynamically manage, price, and hedge counterparty risk. The benefits of CVA are widely acknowledged. Many banks have set up internal credit risk trading desks to manage counterparty risk on derivatives. This presentation answers several fundamental questions: what is CVA? Why does CVA become important? How can one compute CVA? 

Keywords:
Credit value adjustment, CVA, credit risk, valuation, risk management, counterparty risk

	CVA Definition
	CVA is defined as the difference between the risk-free portfolio value and the true/risky portfolio value; or
	CVA is the market price of counterparty credit risk
	In practice, CVA should be computed at portfolio level. That means calculation should take Master agreement and CSA agreement into account.


	CVA Calculation: Credit Exposure Approach
	Model description

CVA=(1-R)∫_0^T▒〖〖EE〗^* (t)dPD(0,t)〗
where 
〖EE〗^* (t) 		the discounted risk-neutral expected credit exposure
R 	the recovery rate
PD 	the risk neutral probability of default


	Pros
	Simple and intuitive
	Make best reuse of the existing counterparty credit exposure system
	Relatively easy to implement
	Cons
	Theoretically unsound
	Inaccurate

	CVA Calculation: Least Square Monte Carlo Approach
	Model description

CVA=V_f (t)-V_r (t)
where 
V_f (t)=E[D(t,T) X_T ] 		the risk free value
 V_r (t)=E[D(t,T)X_T (1-1_(X_T≥0) q(1-R))] 	the risky/true value
D(t,T)	 the risk-free discount factor
q 	the risk neutral survival probability
R 	the recovery rate
 		X_T	the payoff
V_r (t)=E[Y(t,T)X_T ]E=[D(t,T)(1-1_(X_T≥0) q(1-R))]


The least square Monte Carlo approach was introduced by Xiao
	Pros
	Theoretically sound: can be rigorously proved. 
	Accurate valuation
	Valuation is performed by Longstaff-Schwartz least squares Monte Carlo approach.
	Cons
	Calculation procedure is different from credit exposure computation. 
	Hardly reuse the existing credit exposure system.

	Master Agreement
	Master agreement is a document agreed between two parties, which applies to all transactions between them.
	Close out and netting agreement is part of the Master Agreement.
	If two trades can be netted, the credit exposure is
E(t)=max(V_1 (t)+V_2 (t),0)
	If two trade cannot be netted (called non-netting), the credit exposure is
E(t)=max(V_1 (t),0)+max(V_2 (t),0)

	CSA Agreement
	CSA (or Margin Agreement or Collateral Agreement) is a legal document that regulates collateral posting.
	Trades under a CSA should be also under a netting agreement, but not vice verse.
	It defines a variety of terms related to collateral posting.
	Threshold
	Minimum transfer amount (MTA)
	Independent amount (or initial margin or haircut)

	Risk Neutral Simulation: interest rate and FX
	Recommended 1-factor model: Hull-White
 
where
 r 	the yield risk factor
α 	the drift
 θ	the mean reverse
σ 	the volatility 
W  	the Wiener process

	Simple and easy to implement
	Recommended  multi-factor model: 2-factor Hull-White or Libor Market Model (LMM)
	There is a demand in the market to use multi-factor models
	All curve simulation should be brought into a common measure. 
	Simulate interest rate curves in different currencies.
	Change measure from the risk neutral measure of a quoted currency to the risk neutral measure of the base currency.
	Forward FX rate can be derived using interest rate parity
	
F = S e 		F=S_0 exp(r_s-r_q )t

	Risk Neutral Simulation: equity price
	Simulate spot stock price.
	Geometric Brownian Motion (GBM)
 
	Pros
	Simple
	Non-negative stock price
	Cons
	Simulated values could be extremely large for a longer horizon, so it may be better to incorporate with a reverting draft.

	Commodity simulation
	Simulate commodity spot, future and forward prices as well as pipeline spreads
	Two factor model

log⁡(S_t )=q_t+X_t+Y_t
〖dX〗_t=(α_1-γ_1 X_t )dt+σ_1 〖dW〗_t^1
〖dY〗_t=(α_2-γ_2 Y_t )dt+σ_2 〖dW〗_t^2
〖dW〗_t^1 〖dW〗_t^2=ρdt
where	
S_t 	the spot price or spread or implied volatility
	S_t 	the deterministic function
X_t 	the short term deviation
Y_t 	the long term equilibrium level

	The model leads to a closed form solution of forward prices and thus forward term structure.

	Implied volatility simulation
	In risk neutral world, the volatility is embedded in the price simulation.
	Thus, there is no need to simulate implied volatility.

	Credit Exposure Approach Implementation

	Compute the risk-free value V_f (t) of a counterparty portfolio that should be reported by trading systems.
	The solution is based on existing credit exposure framework.
	Switch simulation from the real-world measure to the risk neutral measure.
	Calculate risk-neutral credit exposures (EEs) taking master agreement and CSA into account.
	You can directly compute CVA using the following formula
CVA=(1-R)∑_(k=1)^N▒〖[PD(t_k )-PD(t_(k-1) )] 〖EE〗^* (t)〗
	Or you can compute the risky value V_r (t) of the portfolio via discounting positive EEs by  counterparty’s CDS spread + risk-free interest rate as the positive EEs flows bearing counterparty risk and negative EEs by the bank’s own CDS spread + risk-free interest rate as the negative EEs bearing the bank’s credit risk.
	CVA=V_f (t)-V_r (t)

	Least Square Monte Carlo Approach Implementation

	Compute the risk-free value V_f (t) of a counterparty portfolio that should be reported by trading systems.
	Simulate market risk factors in the risk-neutral measure.
	Generate payoffs for all trades based on Monte Carlo simulation.
	Aggregate payoffs based on the Master agreement and CSA.
	Compute the risky value V_r (t) of the portfolio using Longstaff-Schwartz approach.
	Positive cash flows should be discounted by counterparty’s CDS spread + risk-free interest rate while negative cash flows should be discounted by the bank’s own CDS spread + risk-free interest rate.
	CVA=V_f (t)-V_r (t)


References:

https://finpricing.com/lib/FxVolIntroduction.html
