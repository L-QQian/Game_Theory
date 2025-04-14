from otree.api import *


doc = """
公共財ゲーム実験
"""

class C(BaseConstants):
    NAME_IN_URL = 'public_goods_trial'
    PLAYERS_PER_GROUP = 3
    NUM_ROUNDS = 1
    # 1期のみ
    ENDOWMENT = cu(20)
    # 初期保有額20ポイント
    MULTIPLIER = 1.6
    # 全員の貢献の合計を1.6倍


class Subsession(BaseSubsession):
    pass


class Group(BaseGroup):
    total_contribution = models.CurrencyField()
    # 各グーグルの貢献額の合計を入れるオブジェクト（目的）
    individual_share = models.CurrencyField()
    # 各プレイヤーの個別の分配額を入れるオブジェクト


class Player(BasePlayer):
    contribution = models.CurrencyField(
        # プレイヤーの貢献額を入れるオブジェクト（目的）
        choices = currency_range(cu(0),cu(C.ENDOWMENT), cu(1)), max=C.ENDOWMENT, 
        # 0から初期保有額（20ポイン）までの１ポイント刻みの選択肢を表示する
        label="あなたはいくら貢献しますか"
        # 貢献の選択肢の前に設問文を表示する
    )


# FUNCTIONS
def compute(group:Group):
    players = group.get_players()
    # groupクラスに所属するプレイヤーの情報を取得
    contributions = [p.contribution for p  in players]
    # プレイヤーの貢献額をリストにまとめる
    group.total_contribution = sum(contributions)
    # グーグルの総貢献額を計算
    group.individual_share = (
        group.total_contribution * C.MULTIPLIER / C.PLAYERS_PER_GROUP
        )
    # 各プレイヤーへの分配額を計算
    for p in players:
    # 各プレイヤーへの獲得額を計算
        p.payoff = C.ENDOWMENT - p.contribution + group.individual_share
        # 各プレイヤーのpayoffは初期保有額から貢献額を引いて
        # 各プレイヤーの分配額を加えたものである


# PAGES
class page1(Page):
    form_model = 'player'
    form_fields = ['contribution']
    # プレイヤーが貢献額を入力する


class Page2(WaitPage):
    after_all_players_arrive = compute
    # 全プレイヤーががpage1 で貢献額を入力したあとに compute 関数を実行する


class page3(Page):
    pass


page_sequence = [page1, Page2, page3]
